---
layout: post
title: Mylinkedlist
categories: 数据结构
description: 手搓链表
keywords: C++
---

```c++
#ifndef QW_Mylinklist_H_
#define QW_Mylinklist_H_

#include <assert.h>

template <typename T>
class Mylinklist
{
public:
    struct Node
    {
        T data;
        Node *next;
        Node(T data_ = {}, Node *next_ = nullptr)
            : data(data_), next(next_) {}
    };

public:
    Mylinklist()
        : length(0), rear(&head) {}
    ~Mylinklist()
    {
        clear();
    }

public:
    int size() const
    {
        return length;
    }

    bool empty() const
    {
        return length == 0;
    }

    void push(T item)
    {
        rear->next = new Node(item);
        length++;
        rear = rear->next;
    }

    T pop()
    {
        assert(length != 0);
        Node *temp = &head;
        while (temp->next->next != nullptr)
        {
            temp = temp->next;
        }
        delete temp->next;
        temp->next = nullptr;
        rear = temp;
        length--;
    }

    void clear()
    {
        if (length == 0)
            return;
        Node *prev = head.next;
        while (prev != nullptr)
        {
            Node *temp = prev->next;
            delete prev;
            prev = temp;
        }
        head.next = nullptr; 
        length = 0;
    }

    template <typename Functor>
    void foreach (Functor f)
    {
        Node *iter = head.next;
        while (iter != nullptr)
        {
            f(iter->data);
            iter = iter->next;
        }
    }

private:
    int length;
    Node *rear;
    Node head;
};

#endif /* QW_Mylinklist_H_ */
```