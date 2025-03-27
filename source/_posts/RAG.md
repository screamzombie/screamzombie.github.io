---
title: RAG检索增强生成
categories: LLM
---

你的部队每时每刻都在伤亡,我的虫群无穷无尽

<!--more-->

# Hello World


一个简单的RAG的例子


```python3
import re
import jieba
import torch
import numpy as np
import requests
from typing import List, Tuple
from rank_bm25 import BM25Okapi
from langchain_core.documents import Document
from langchain_community.vectorstores import FAISS
from langchain_huggingface import HuggingFaceEmbeddings
from transformers import AutoTokenizer, AutoModelForSequenceClassification


class DeepSeekLLM:
    def __init__(self, api_key: str, api_url: str = "https://api.deepseek.com/v1/chat/completions"):
        self.api_key = api_key
        self.api_url = api_url

    def generate(self, query: str, docs: List[str]) -> str:
        context = "\n\n".join(docs)
        prompt = (
            "你是一个精通魔法道具的助手，请根据提供的资料回答用户的问题。\n"
            f"用户问题：{query}\n\n"
            f"相关资料：\n{context}\n\n"
            "请根据以上资料，用简洁明了的语言回答用户的问题。"
        )
        payload = {
            "model": "deepseek-chat",
            "messages": [{"role": "user", "content": prompt}],
            "temperature": 0.7
        }
        headers = {
            "Authorization": f"Bearer {self.api_key}",
            "Content-Type": "application/json"
        }
        response = requests.post(self.api_url, headers=headers, json=payload)
        result = response.json()
        return result["choices"][0]["message"]["content"]

    def generate_without_context(self, query: str) -> str:
        prompt = (
            "你是一个精通魔法道具的助手，请回答下列问题：\n"
            f"{query}\n"
            "请尽可能根据你的知识进行回答。"
        )
        payload = {
            "model": "deepseek-chat",
            "messages": [{"role": "user", "content": prompt}],
            "temperature": 0.7
        }
        headers = {
            "Authorization": f"Bearer {self.api_key}",
            "Content-Type": "application/json"
        }
        response = requests.post(self.api_url, headers=headers, json=payload)
        result = response.json()
        return result["choices"][0]["message"]["content"]


# 1. 加载知识库
def parse_herb_txt(file_path: str) -> List[str]:
    with open(file_path, "r", encoding="utf-8") as f:
        raw_text = f.read()

    entries = raw_text.strip().split("-")
    cleaned_chunks = []

    for entry in entries:
        lines = [line.strip() for line in entry.strip().splitlines() if line.strip()]
        if not lines:
            continue

        herb_name = re.sub(r"[{}]", "", lines[0])
        content = "\n".join(lines[1:])
        full_text = f"【药名】：{herb_name}\n{content}"
        cleaned_chunks.append(full_text)

    return cleaned_chunks


# 2. 重排序模型（BGE）
class BGEReranker:
    def __init__(self, model_name="BAAI/bge-reranker-base"):
        self.tokenizer = AutoTokenizer.from_pretrained(model_name)
        self.model = AutoModelForSequenceClassification.from_pretrained(model_name)
        self.model.eval()

    def rerank(self, query: str, docs: list[str]) -> list[tuple[str, float]]:
        pairs = [(query, doc) for doc in docs]
        inputs = self.tokenizer(pairs, padding=True, truncation=True, return_tensors="pt", max_length=512)

        with torch.no_grad():
            scores = self.model(**inputs).logits.squeeze().tolist()

        if isinstance(scores, float):
            scores = [scores]

        ranked = sorted(zip(docs, scores), key=lambda x: x[1], reverse=True)
        return ranked


# 3. 主检索器（向量 + BM25 多路召回）
class EnhancedRetriever:
    def __init__(self, chunks: List[str]):
        self.chunks = chunks
        self.documents = [Document(page_content=chunk) for chunk in chunks]
        # 向量检索初始化
        self.embeddings = HuggingFaceEmbeddings(
            model_name="shibing624/text2vec-base-chinese",
            model_kwargs={"device": "cpu"},
            encode_kwargs={"normalize_embeddings": False}
        )
        self.vector_db = FAISS.from_documents(self.documents, self.embeddings)
        # BM25 初始化
        self.tokenized_chunks = [list(jieba.cut(doc)) for doc in chunks]
        self.bm25 = BM25Okapi(self.tokenized_chunks)
        self.reranker = BGEReranker()

    def search(self, query: str, vector_k=5, bm25_k=5) -> List[Tuple[str, float]]:
        # 向量检索
        vector_results = self.vector_db.similarity_search_with_score(query, k=vector_k)
        vector_docs = [doc[0].page_content for doc in vector_results]

        # BM25 检索
        bm25_tokens = list(jieba.cut(query))
        bm25_scores = self.bm25.get_scores(bm25_tokens)
        top_bm25_indices = np.argsort(bm25_scores)[::-1][:bm25_k]
        bm25_docs = [self.chunks[i] for i in top_bm25_indices]

        # 合并结果 + 去重
        combined_docs = list(dict.fromkeys(vector_docs + bm25_docs))  # 去重保序

        # 用 BGE 重排
        reranked = self.reranker.rerank(query, combined_docs)
        return reranked


# 4. 主流程
if __name__ == "__main__":
    LLM = DeepSeekLLM(api_key="sk-b8407fdb04ab4546bf166e9ddef4a7f6")
    HERB_TXT_PATH = "queer.txt"
    print(" 正在加载知识库...")
    chunks = parse_herb_txt(HERB_TXT_PATH)
    print(f" 成功载入 {len(chunks)} 个知识条目")

    retriever = EnhancedRetriever(chunks)

    while True:
        query = input("\n 请输入你的问题（输入 q 退出）：").strip()
        if query.lower() == "q":
            break

        results = retriever.search(query, vector_k=10, bm25_k=10)

        print("检索结果评分如下（已重排序）：")
        for i, (doc, score) in enumerate(results):
            print(f"[{i+1}] 分数: {score:.4f}")
            print(doc)
            print("-" * 60)

        # 选前3条用于生成回答
        top_docs = [doc for doc, _ in results[:3]]
        print("正在生成回答")
        rag_answer = LLM.generate(query, top_docs)
        raw_answer = LLM.generate_without_context(query)    
        print("RAG模型回答：")
        print(rag_answer)
        print("原始模型回答（无上下文）：")
        print(raw_answer)
```