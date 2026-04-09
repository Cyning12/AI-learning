# AI-learning（RAG + FAISS）

本仓库用于学习与沉淀 **RAG（Retrieval-Augmented Generation）** 实践：从多来源 PDF/文档中解析结构化文本，切分为带元数据的 chunks，生成 embedding，并使用 **FAISS** 做向量检索，最终提供可查询的知识库能力。

## 目标与范围

- **核心目标**：将“公司规章/制度”等 PDF 文档构建为可检索知识库，支持 Top-k 相似度召回与答案生成。
- **技术栈**：Python 3.10+、FAISS、LangChain（或 LlamaIndex）、PyMuPDF（可选 PDFPlumber）、asyncio。
- **关键约束**：
  - **内存效率**：面向大批量 PDF，按页/流式处理，及时释放资源。
  - **元数据完整性**：每个 chunk 必须保留 `filename`、`page_number`、`section_header` 等来源信息，便于溯源与调试。
  - **索引可持久化**：FAISS index 与相关元数据落盘，避免每次重建。

## 目录结构（当前）

- `课程练习/`：课程配套练习代码与实验
- `学习日记/`：学习记录
- `requirements.txt`：Python 依赖

> 后续建议新增（需要时再补齐）：`src/`（核心代码）、`data/`（原始文档与处理产物）、`scripts/`（命令行脚本）、`tests/`（最小回归）。

## 环境准备

### 1) 创建虚拟环境

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -U pip
```

### 2) 安装依赖

```bash
pip install -r requirements.txt
```

## 配置（重要）

本项目使用 `.env` 存放密钥/环境变量；**禁止提交到 Git**（已在 `.gitignore` 中忽略）。

建议 `.env`（示例）：

```bash
# LLM/Embedding 提供方（按你实际使用的 SDK 变量名调整）
OPENAI_API_KEY=your_key
OPENAI_BASE_URL=https://api.openai.com/v1
```

## 最小跑通（占位说明）

当前仓库已有练习脚本，后续当你把核心 RAG 流程模块化到 `src/` 后，建议提供以下最小可验证命令：

- **解析 PDF -> chunks（带元数据）**
- **chunks -> embeddings -> FAISS 持久化**
- **query -> Top-k 检索 -> 生成答案（含来源引用）**

为了便于 Git 历史清晰，建议每一步都产出：
- 可复现的命令
- 输出路径约定（如 `data/processed/`、`data/index/`）
- 最小日志（包含文档名、页码、chunk id、Top-k 命中信息）

## Git 提交建议

- **不提交**：`.env`、`.DS_Store`、虚拟环境目录（`.venv/`）、体积过大的原始数据与中间产物（可用小样本/抽样数据替代）。
- **提交**：代码、必要的小样本数据（如 1-2 个脱敏 PDF/或已转换的 markdown）、可复现脚本与说明文档。

## 许可证

如后续要开源发布，建议补充 `LICENSE`（MIT/Apache-2.0 等）。目前未指定许可证。

