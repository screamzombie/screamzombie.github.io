---
title: TypeScript学习
categories: 前端
---

Hello TypeScript

<!-- more -->

# TypeScript 基础

## 如何在终端运行 Ts

1. 安装 ts

```shell
   npm install -g typescript
```

2. 将 .ts 文件编译成 .js 文件

```shell
tsc test.ts // 会在当前目录下生成对应的 test.js 文件
```

3.  运行 .js 文件

```shell
node test.js
```

也可以使用`ts-node`直接运行`.ts`文件

```shell
npm install -g typescript ts-node
ts-node test.ts
```
