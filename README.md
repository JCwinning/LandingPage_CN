[English](README.md) | [中文](_README_CN.md)

# AI_Blog

一个基于 Quarto 构建的技术博客，展示 AI/ML 项目、教程和交互式应用程序。本项目既是作品集，也是面向 AI 爱好者和开发者的教育资源。

**在线访问：** [https://jcwinning.github.io/LandingPage/](https://jcwinning.github.io/LandingPage/)


## 简介

AI_Blog 是使用 [Quarto](https://quarto.org/) 构建的静态网站，特色包括：

- **10+ AI/ML 项目教程** - OpenRouter API、RAG 实现、AI 聊天应用等全面指南
- **交互式代码示例** - 可执行的 Python 和 R 代码块，实时演示
- **双语内容** - 全面的中英文支持
- **数据可视化** - 使用 Plotly 和 Streamlit 构建的交互式仪表板
- **专业作品集** - 展示技术专长的简历页面

## 精选项目

| 项目 | 描述 | 技术栈 |
|------|------|--------|
| [OpenRouter 教程](posts/openrouter/) | OpenRouter API 多模型访问综合指南 | Python, OpenAI SDK |
| [RAG 实现](posts/RAG/) | R & Python 实现检索增强生成 | LlamaIndex, ChromaDB, DuckDB |
| [AI 聊天应用](posts/AI-Chat/) | 多语言多模型聊天应用 | Streamlit, OpenRouter |
| [GDP 仪表板](posts/gdp-trend/) | 交互式 GDP 趋势与 AI 驱动 SQL | Streamlit, DuckDB, Plotly |
| [LLM 摘要](posts/llm-summary/) | 多平台 AI 摘要工具 | OpenAI, ModelScope |
| [YOLO 目标检测](posts/yolo-app/) | 实时目标检测应用 | YOLO11, OpenCV |
| [天气可视化](posts/weather-trend/) | 天气数据分析和趋势 | Pandas, Plotly |
| [商店地图管理](posts/shop-map-manager/) | 商店位置管理系统 | Streamlit, Folium |

## 技术栈

### 核心框架
- **Quarto** - 从 Markdown 创建网站的科学发布系统

### AI/ML
- **OpenRouter API** - 多 AI 模型统一 API
- **OpenAI API** - GPT 模型和嵌入
- **LangChain** - LLM 应用框架
- **LlamaIndex** - LLM 应用的数据框架
- **ChromaDB** - 向量数据库
- **DuckDB** - 支持向量搜索的内存 SQL 数据库
- **Whisper (MLX)** - 语音转文字转录
- **YOLO11** - 目标检测模型

### 数据与可视化
- **Pandas** - 数据处理
- **Plotly** - 交互式可视化
- **Streamlit** - Web 应用框架
- **wbgapi** - 世界银行 API

### 编程语言
- **Python** - 主要实现语言
- **R** - 统计计算和图形
- **Markdown** - 内容创作
- **CSS** - 自定义样式

## 快速开始

### 前置要求

1. **Quarto CLI** - 从 [quarto.org](https://quarto.org/docs/get-started/) 安装
2. **Python 3.12+** - 用于代码示例和 AI 实现
3. **R 4.0+**（可选）- 用于基于 R 的文章

### 安装

```bash
# 克隆仓库
git clone https://github.com/jcwinning/AI_Blog.git
cd AI_Blog

# 安装 Python 依赖（以 OpenRouter 文章为例）
pip install openai python-dotenv pandas IPython panel

# 安装 R 包（可选，用于基于 R 的文章）
# 在 R 控制台中运行：
install.packages(c("ragnar", "ellmer", "reticulate", "rvest", "dotenv"))
```

### 配置

在项目根目录创建 `.env` 文件配置 API 密钥：

```bash
OPENROUTER_API_KEY=your_key_here
MODELSCOPE_API_KEY=your_key_here
OPENAI_API_KEY=your_key_here
```

### 本地运行

```bash
# 本地预览网站
quarto preview

# 或渲染到 docs 文件夹
quarto render
```

访问 `http://localhost:4000` 查看网站

## 项目结构

```
AI_Blog/
├── _quarto.yml          # Quarto 主配置文件
├── index.qmd            # 首页
├── posts.qmd            # 博客列表页
├── cv.qmd               # 简历页面
├── styles.css           # 自定义 CSS 样式
├── posts/               # 博客文章目录
│   ├── openrouter/      # OpenRouter API 教程
│   ├── RAG/             # RAG 实现
│   ├── AI-Chat/         # AI 聊天应用
│   ├── gdp-trend/       # GDP 仪表板
│   └── ...
├── markdown_docs/       # 文档参考
├── docs/                # 生成的网站输出
├── _freeze/             # 计算缓存
└── posts_files/         # 文章资源
```

## 部署

网站部署在 GitHub Pages 上。部署方法：

```bash
# 渲染网站
quarto render

# 提交并推送到 GitHub
git add docs/
git commit -m "更新网站"
git push
```


## 贡献

欢迎贡献！添加新博客文章：

1. 在 `/posts/` 中创建新目录
2. 添加带有正确 YAML frontmatter 的 `.qmd` 或 `.md` 文件
3. 在子目录中包含相关资源
4. 如需要，更新 `_quarto.yml` 中的导航
5. 使用 `quarto preview` 本地渲染和测试

## 许可证

本项目是开源的，采用 MIT 许可证。

## 作者

**Tony D** - [GitHub](https://github.com/jcwinning)

## 致谢

- [Quarto](https://quarto.org/) 提供的优秀发布系统
- 开源 AI/ML 社区的工具和库
- 所有贡献者和支持者
