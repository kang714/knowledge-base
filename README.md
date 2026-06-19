# 📚 本地知识库系统

一个基于 RAG（检索增强生成）的本地知识库管理系统，支持文档上传、语义搜索和智能问答。

## ✨ 功能

- **📄 文档管理**：上传、查看、编辑、删除文档（txt/md/pdf/docx）
- **🔍 语义搜索**：基于向量相似度的智能搜索
- **💬 智能问答**：基于知识库内容的 RAG 问答
- **⚙️ 灵活配置**：支持 Ollama 本地模型或 sentence-transformers 纯本地嵌入
- **🎯 完全本地**：所有数据和处理都在本地完成，无需联网

## 🚀 快速开始

### 1. 环境要求

- Python 3.11+
- [Ollama](https://ollama.com)（可选，推荐）

### 2. 安装依赖

```bash
cd E:\知识库
pip install -r requirements.txt
```

### 3. 安装 Ollama 模型（推荐）

```bash
# 启动 Ollama（另一个终端）
ollama serve

# 拉取模型
ollama pull qwen2.5:latest         # LLM 模型（用于问答）
ollama pull nomic-embed-text       # 嵌入模型（用于搜索）
```

> 如果不想安装 Ollama，系统会自动使用 sentence-transformers 作为嵌入模型后备。但问答功能需要 Ollama。

### 4. 启动应用

```bash
python run.py
```

浏览器打开 http://localhost:8501

## 📖 使用指南

### 上传文档

1. 进入「📄 文档管理」→「上传文档」
2. 选择文件（支持 txt, md, pdf, docx）
3. 点击「开始上传并处理」
4. 系统会自动解析、分块、嵌入和索引

### 搜索

1. 进入「🔍 语义搜索」
2. 输入关键词或问题
3. 系统使用语义匹配找到最相关的文档内容

### 问答

1. 进入「💬 智能问答」
2. 输入问题
3. 系统会从知识库中检索相关内容，并用 LLM 生成答案
4. 答案会附带参考来源

### 设置

- 进入「⚙️ 设置」查看 Ollama 状态
- 检查模型是否已安装
- 修改 `config.yaml` 调整分块大小、模型等参数

## ⚙️ 配置说明

编辑 `config.yaml` 自定义配置：

```yaml
llm:
  model: qwen2.5:latest      # 问答模型
  temperature: 0.7
  max_tokens: 2048

embeddings:
  provider: ollama            # ollama 或 sentence_transformer
  model: nomic-embed-text     # 嵌入模型

chunking:
  chunk_size: 1000            # 分块大小（字符）
  chunk_overlap: 200          # 块重叠（字符）
```

## 📁 项目结构

```
E:\知识库\
├── config.yaml          # 配置文件
├── run.py               # 启动入口
├── requirements.txt     # 依赖
├── app/                 # 应用代码
│   ├── main.py          # Streamlit 主程序
│   ├── config.py        # 配置加载
│   ├── database/        # 数据库层
│   ├── embeddings/      # 嵌入模型
│   ├── llm/             # 大模型
│   ├── ingestion/       # 文件解析
│   ├── search/          # 向量检索
│   ├── services/        # 业务服务
│   └── ui/              # 界面
│       ├── components/  # 组件
│       └── pages/       # 页面
└── data/                # 运行时数据
    ├── documents/       # 原始文件
    ├── chroma/          # 向量库
    └── knowledge.db     # 数据库
```

## 🔧 常见问题

**Q: Ollama 连接失败？**
A: 确保已启动 Ollama 服务：`ollama serve`

**Q: 没有安装模型？**
A: 运行 `ollama pull qwen2.5:latest` 和 `ollama pull nomic-embed-text`

**Q: PDF 文本提取为空？**
A: 部分扫描版 PDF 不含文本层，需要 OCR。推荐使用可选中文本的 PDF。

**Q: 可以不用 Ollama 吗？**
A: 嵌入功能可以使用 sentence-transformers（自动后备）。但问答功能需要 Ollama 提供 LLM。
