+++
title = 'LLM Wiki 本地搭建与使用调研'
date = '2026-08-20T16:10:00+08:00'
draft = false
tags = ['LLM', 'AI']
slug = 'llm-wiki'
+++

# LLM wiki本地搭建+使用调研

> 本文是对 LLM Wiki 的完整调研记录，涵盖产品介绍、两种本地部署方案与核心能力验证，适合希望自建个人或团队知识库的开发者参考。

调研知识提取、结构化转换、实体链接、增量更新等能力，如何使用，是否可以作为AI Agent的输入（提示词、记忆、规则等），是否可以做成团队使用

## 1. 📌 产品简介
### 1.1 📖 产品概述
**LLM全称‌大语言模型（Large Language Model）‌，是基于深度学习技术构建的超大规模语言模型，它通过对海量文本数据进行自监督学习，掌握自然语言的语法、语义、逻辑规律，能模拟人类的语言理解和生成过程。**
这类模型通常拥有数百亿级以上的参数，依托Transformer架构的自注意力机制，不仅能完成拼写检查、文本摘要、机器翻译等基础NLP任务，还具备复杂逻辑推理、代码生成、专业领域内容分析等高阶能力，是当前生成式AI的核心技术底座。
**Wiki（音译“维基”）是一种支持多人协同创作的超文本知识系统，最早由沃德·坎宁安在1995年提出，名称源自夏威夷语中“快速”的含义。**
它的核心特点是：所有页面通过双向链接互相关联，支持用户自由创建、编辑内容，同时自动记录每一次修改的版本历史，实现知识的持续迭代沉淀。最典型的应用是维基百科，除此之外它也被广泛用于企业内部知识库、项目文档协作、个人知识管理等场景。
**LLM Wiki是2026年4月由OpenAI创始团队成员Andrej Karpathy提出的‌新一代个人知识库构建范式‌，它彻底改变了传统RAG“查询时临时检索、查完即忘”的模式，把大语言模型从“临时解释器”升级为“知识编译器”。**
**LLM Wiki 是一款面向大语言模型（LLM）的知识管理系统，支持本地部署，可将企业内部文档（如 PDF、Word、Markdown、HTML 等）转换为可供大语言模型检索和理解的知识库。系统结合向量检索（RAG）技术，实现知识的统一管理、智能检索和问答，可作为 AI Agent 的知识来源。**
**💡 核心逻辑‌**
它会提前将零散的原始资料（论文、笔记、网页等）交给大模型，“编译”生成结构化、高关联的Wiki知识库，后续查询时直接调用已经沉淀好的知识网络，无需每次重新从原始文档检索拼接，解决了传统RAG没有知识积累、重复计算的痛点。
**🏗️ 标准三层架构‌**
原始资料区：存放所有未经修改的源文件，采用只读策略，作为知识的唯一可追溯基准
LLM生成Wiki区：由大模型自动维护的Markdown知识库，包含概念摘要、交叉链接网络、矛盾点标注等内容
规则配置区：定义大模型处理资料、生成页面、响应查询的行为规范，把通用大模型调教为专属知识管理员
**⭐ 核心优势‌**
随着新资料不断加入，知识库会自动更新关联旧内容，实现知识的指数级复利增长；相比传统RAG，Token消耗最高可降低95%，运行效率提升70倍，且所有生成内容都是标准Markdown格式，完全支持人工直接阅读和编辑。
目前社区已经衍生出多个成熟的落地项目，覆盖桌面端应用、Python开发库等不同形态，可用于学术研究、个人学习、企业文档管理等各类知识沉淀场景。
### 1.2 🎯 产品定位
LLM Wiki 的定位是企业级知识库平台，主要解决以下问题：
- 企业文档统一管理 
- 非结构化知识结构化处理 
- 基于知识库的智能问答 
- 为 AI Agent 提供知识输入 
- 支持本地私有化部署，保障数据安全
### 1.3 ⚙️ 核心功能
主要功能包括：
- 文档导入（PDF、Word、Markdown 等） 
- 自动知识切分（Chunk） 
- 向量化存储（Embedding） 
- 知识检索（RAG） 
- 基于知识库的问答 
- 权限管理（如支持） 
- 增量更新（如支持） 
- API 接口（如支持）
## 2. 🚀 本地部署过程
### 🐳 AnythingLLM+Ollama
![示意图](images/image1.png)
#### 2.1.1 🐳 安装Docker Desktop
https://www.docker.com/products/docker-desktop/
验证：cmd-->docker --version-->看到类似Docker version 28.xx，成功
#### 2.1.2 🦙 安装Ollama
https://ollama.com
验证：cmd-->ollama-->出现帮助信息，成功
#### 2.1.3 ⬇️ 下载模型
cmd-->ollama pull qwen2.5:7b-->ollama list-->qwen2.5:7b，成功
#### 2.1.4 🚢 部署AnythingLLM
1.创建目录D:\AnythingLLM
2.执行docker pull mintplexlabs/anythingllm
3.运行docker run -d ^
--name anythingllm ^
-p 3001:3001 ^
-v anythingllm:/app/server/storage ^
mintplexlabs/anythingllm
浏览器打开http://localhost:3001，看到登录页面说明部署成功
#### 2.1.5 🔌 连接Ollama
LLM provider：Ollama
模型：qwen2.5:7b
地址：http://host.docker.internal:11434
![示意图](images/image2.png)
### 2.2 📓 Obsidian+Hermes
#### 2.2.1 ⬇️ 下载Obsidian
https://obsidian.md/zh/download
#### 2.2.2 ⬇️ 下载LLM
https://github.com/nashsu/llm_wiki/releases
#### 2.2.3 🛠️ 本地LLM部署
打开安装的LLM，新建文件
![示意图](images/image3.png)
填写名称，选择语言和文件路径
![示意图](images/image4.png)
设置模型，输入API Key
![示意图](images/image5.png)
导入文件，就可以对文件内容进行解析
![示意图](images/image6.png)
#### 2.2.4 📓 Obsidian
打开仓库管理选择刚才LLM新建的文件，就能看到LLM做了哪些操作
![示意图](images/image7.png)
### 2.3 ⚖️ 对比
![示意图](images/image8.png)
## 3. 🧩 基础功能
一套基于Markdown知识库和Agent自动管理能力的个人LLM Wiki系统
Hermes：AI Agent框架，负责执行任务和操作知识库，可以接收用户任务、调取本地LLM、操作Obsidian Vault、自动生成和维护Wiki页面
Obsidian：知识展示和存储层，所有笔记以.md存放、双向链接、关系知识图谱、全文快速检索
## 4. 🔍 知识提取能力
(PDF/Word/Markdown/图片OCR)
能够从原始Markdown文档中识别主要医学实体，包括疾病、解剖结构、训练方法等，并生成对应知识页面。
![示意图](images/image9.png)
![示意图](images/image10.png)
## 5. 🔄 结构化转换能力
(Chunk段落/Heading标题/Metadata元数据/Tag标签)
不要直接结构化的 Markdown，而是给它一段普通的、连续叙述式文本，模拟真实场景中的知识输入
![示意图](images/image11.png)
自动转换成wiki结构
![示意图](images/image12.png)
![示意图](images/image13.png)
## 6. 🧲 检索能力
(关键词/向量/Hybrid混合)
![示意图](images/image14.png)
## 7. ♻️ 增量更新
(覆盖/版本/自动同步)
![示意图](images/image15.png)
![示意图](images/image16.png)
添加完此文档生成Wiki后，原ACL康复方法中自动更新
![示意图](images/image17.png)
## 8. 🔗 外部连接
（GitHub/Notion/Web）
![示意图](images/image18.png)
## 9. 🤖 AI Agent集成能力
（Prompt提示/Memory记忆/Rule规则/Tool Calling工具调用）
### 9.1 💬 作为prompt提示：
![示意图](images/image19.png)
![示意图](images/image20.png)
### 9.2 🧠 作为memory记忆：
**❌ 无memory：**
![示意图](images/image21.png)
![示意图](images/image22.png)
**✅ 有memory：**
![示意图](images/image23.png)
![示意图](images/image24.png)
但是这里的memory并不是自主形成的，也就是说Hermes当前具备基于会话上下文的短期记忆能力，可以记录用户偏好并影响当前回答。但跨会话长期Memory需要额外的持久化存储机制，例如Markdown文件、数据库或向量数据库。
![示意图](images/image25.png)
![示意图](images/image26.png)
![示意图](images/image27.png)
### 9.3 📏 作为rules规则：
![示意图](images/image28.png)
![示意图](images/image29.png)

## 📌 总结

LLM Wiki 的核心思路是把原始资料提前"编译"成结构化、高关联的 Markdown 知识库，供大模型直接检索使用，区别于传统 RAG"查询时才临时检索"的模式。本次调研覆盖了两种本地部署方案（AnythingLLM + Ollama、Obsidian + Hermes）以及知识提取、结构化转换、检索、增量更新、外部连接和 AI Agent 集成等核心能力，整体适合作为个人或团队知识库的落地方案。

| 部署方案 | 组件 | 适用场景 |
| --- | --- | --- |
| AnythingLLM + Ollama | AnythingLLM（Web 界面）+ Ollama（本地大模型） | 快速搭建、开箱即用 |
| Obsidian + Hermes | Obsidian（知识展示/存储）+ Hermes（AI Agent 框架） | 深度定制、与笔记工作流结合 |

| 核心能力 | 说明 |
| --- | --- |
| 知识提取 | 支持 PDF、Word、Markdown、图片 OCR |
| 结构化转换 | Chunk 切分、Heading 标题、Metadata 元数据、Tag 标签 |
| 检索 | 关键词、向量、Hybrid 混合检索 |
| 增量更新 | 覆盖、版本、自动同步 |
| AI Agent 集成 | 可作为 Prompt、Memory、Rule、Tool Calling 的输入 |
