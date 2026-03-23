# Claude Code 入门指南

> 从零开始配置 Claude Code，用 AI 做 Vibe Coding

本教程帮助你快速上手 [Claude Code](https://docs.anthropic.com/claude/docs/claude-code)——Anthropic 官方的 AI 编程 CLI 工具。你将学会：

- 安装并配置 Claude Code
- 获取 Anthropic API Key
- 用 Claude Code 做 **Vibe Coding**（与 AI 对话写代码）
- 安装增强 Skills，解锁更多工作流

---

## 目录

1. [前置条件](#1-前置条件)
2. [安装 Claude Code](#2-安装-claude-code)
3. [配置 API Key](#3-配置-api-key)
4. [快速上手 Vibe Coding](#4-快速上手-vibe-coding)
5. [安装 Skills](#5-安装-skills)
6. [Skills 使用指南](#6-skills-使用指南)
7. [进阶配置](#7-进阶配置)
8. [常见问题](#8-常见问题)

---

## 1. 前置条件

| 依赖 | 版本要求 | 说明 |
|------|----------|------|
| Node.js | ≥ 18.0 | [下载地址](https://nodejs.org/) |
| npm | 随 Node.js 附带 | - |
| Git | 任意版本 | [下载地址](https://git-scm.com/) |
| Python | ≥ 3.8（可选） | 部分 Skills 需要 |

验证安装：

```bash
node --version   # v18.0.0+
npm --version    # 9.x+
git --version    # 任意版本即可
```

---

## 2. 安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

验证安装：

```bash
claude --version
```

> **Windows 用户**：推荐在 Git Bash 或 WSL2 中使用 Claude Code，体验更好。

---

## 3. 配置 API Key

### 3.1 获取 API Key

1. 访问 [Anthropic Console](https://console.anthropic.com/)
2. 注册/登录账号
3. 进入 **API Keys** 页面
4. 点击 **Create Key**，复制生成的 key

> API Key 格式：`sk-ant-api03-...`

### 3.2 设置环境变量

**macOS / Linux / Git Bash：**
```bash
export ANTHROPIC_API_KEY="sk-ant-api03-你的key"

# 永久生效（加入 ~/.bashrc 或 ~/.zshrc）
echo 'export ANTHROPIC_API_KEY="sk-ant-api03-你的key"' >> ~/.bashrc
source ~/.bashrc
```

**Windows (PowerShell)：**
```powershell
$env:ANTHROPIC_API_KEY = "sk-ant-api03-你的key"

# 永久生效
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "sk-ant-api03-你的key", "User")
```

### 3.3 首次启动

```bash
claude
```

首次运行会引导你完成授权配置。

---

## 4. 快速上手 Vibe Coding

**Vibe Coding** = 用自然语言描述你想要的，让 Claude 直接写代码。

### 4.1 打开项目

```bash
cd 你的项目目录
claude
```

### 4.2 基本对话示例

```
你：帮我写一个 Python 函数，读取 CSV 文件并统计每列的缺失值数量

Claude：[直接写出代码，解释逻辑，询问是否需要调整]

你：加上异常处理，并输出百分比

Claude：[修改代码，保持已有逻辑不变]
```

### 4.3 实用快捷键

| 快捷键 | 功能 |
|--------|------|
| `Enter` | 发送消息 |
| `Shift + Enter` | 换行（不发送） |
| `Ctrl + C` | 中断当前操作 |
| `/help` | 查看所有命令 |
| `/clear` | 清除对话历史 |
| `/exit` | 退出 Claude Code |

### 4.4 斜杠命令

```bash
/help          # 查看帮助
/status        # 查看当前状态
/compact       # 压缩对话历史（节省 token）
/model         # 切换模型
```

### 4.5 Vibe Coding 最佳实践

**✅ 好的提示方式：**
- "帮我重构这个函数，让它更易读，但保持接口不变"
- "这段代码有 bug，输入 `[1,2,3]` 时报 IndexError，帮我找原因"
- "用 TypeScript 写一个 React 组件：输入框 + 提交按钮，点击后调用 `/api/submit`"

**❌ 避免：**
- 描述过于模糊（"帮我优化代码"）
- 一次要求做太多不相关的事
- 不告诉 Claude 你的项目技术栈

---

## 5. 安装 Skills

Skills 是对 Claude Code 的功能扩展，以自定义 `/命令` 的形式运行。

### 5.1 Skills 目录

Skills 安装在 `~/.claude/skills/` 目录下，每个 skill 是一个子目录，包含 `SKILL.md` 和可选的脚本文件。

### 5.2 安装方式

**方法一：克隆本仓库中的 Skills**

```bash
# 克隆本仓库
git clone https://github.com/Whalefall-LSH/claude-code-guide.git
cd claude-code-guide

# 将 skills 复制到 Claude Code 配置目录
cp -r skills/* ~/.claude/skills/
```

**方法二：手动安装单个 Skill**

```bash
# 以 start-my-day 为例
mkdir -p ~/.claude/skills/start-my-day
# 将 SKILL.md 和 scripts/ 复制进去
```

### 5.3 本仓库包含的 Skills

| Skill | 命令 | 功能 |
|-------|------|------|
| `start-my-day` | `/start-my-day` | 每日 arXiv 论文推荐 |
| `paper-analyze` | `/paper-analyze` | 单篇论文深度分析 |
| `paper-search` | `/paper-search` | 搜索已有论文笔记 |
| `conf-papers` | `/conf-papers` | 顶会论文推荐（CVPR/ICLR 等） |
| `extract-paper-images` | `/extract-paper-images` | 提取论文图片 |

### 5.4 Skills 依赖配置

论文相关 Skills 需要额外配置：

**a. 设置 Obsidian Vault 路径（必需）**

```bash
# macOS/Linux
export OBSIDIAN_VAULT_PATH="/path/to/your/obsidian/vault"

# Windows (Git Bash)
export OBSIDIAN_VAULT_PATH="D:/ObsidianVault"
```

建议加入 shell 配置文件（`~/.bashrc`）永久生效。

**b. 创建研究兴趣配置文件（`start-my-day` 需要）**

路径：`$OBSIDIAN_VAULT_PATH/99_System/Config/research_interests.yaml`

```yaml
language: "zh"    # 输出语言：zh 或 en

research_areas:
  - name: "大语言模型"
    keywords:
      - "large language model"
      - "LLM"
      - "GPT"
      - "instruction tuning"
    arxiv_categories:
      - "cs.CL"
      - "cs.AI"

  - name: "多模态"
    keywords:
      - "multimodal"
      - "vision language"
      - "CLIP"
    arxiv_categories:
      - "cs.CV"
      - "cs.MM"
```

**c. 安装 Python 依赖（`start-my-day` / `conf-papers` 需要）**

```bash
pip install pyyaml requests
```

---

## 6. Skills 使用指南

### 6.1 `start-my-day` — 每日论文推荐

每天早上运行，自动从 arXiv 搜索最新论文，生成 Obsidian 推荐笔记。

```bash
# 基本用法（生成今天的推荐）
/start-my-day

# 指定日期
/start-my-day 2026-03-20
```

**输出**：在 Obsidian vault 的 `10_Daily/` 目录生成 `YYYY-MM-DD论文推荐.md`，包含：
- 今日概览（研究趋势、热点分析、阅读建议）
- 前3篇论文：深度分析 + 图片 + 详细报告
- 其余论文：基本信息摘要

### 6.2 `paper-analyze` — 深度分析论文

对单篇论文做全面分析，生成图文并茂的 Obsidian 笔记。

```bash
# 用 arXiv ID
/paper-analyze 2312.12456

# 用论文标题关键词
/paper-analyze "Attention is All You Need"
```

**输出**：在 `20_Research/Papers/[领域]/` 下生成详细笔记，包含：
- 论文背景、问题定义
- 方法架构（含图片）
- 实验结果分析
- 与相关工作对比

### 6.3 `paper-search` — 搜索论文笔记

在已整理的论文笔记中快速搜索。

```bash
# 关键词搜索
/paper-search "contrastive learning"

# 搜索特定作者
/paper-search "Yann LeCun"

# 组合搜索
/paper-search "多模态" "检索增强"
```

### 6.4 `conf-papers` — 顶会论文推荐

搜索顶级学术会议（CVPR/ICLR/NeurIPS/ICML/AAAI/ICCV/ECCV）的高引用论文。

```bash
# 使用默认配置（年份和会议从 conf-papers.yaml 读取）
/conf-papers

# 指定年份
/conf-papers 2024

# 指定年份和会议
/conf-papers 2024 ICLR,NeurIPS
/conf-papers 2023 CVPR
```

**配置文件**：`~/.claude/skills/conf-papers/conf-papers.yaml`
```yaml
keywords:
  - "large language model"
  - "multimodal"
excluded_keywords:
  - "3D"
  - "survey"
default_year: 2024
default_conferences:
  - "ICLR"
  - "NeurIPS"
  - "ICML"
top_n: 10
```

### 6.5 `extract-paper-images` — 提取论文图片

从 arXiv 论文中提取高质量图片（优先从源码包获取，而非 PDF）。

```bash
/extract-paper-images 2312.12456
```

**输出**：图片保存至 `20_Research/Papers/[领域]/[论文标题]/images/`，并生成 `index.md` 索引。

> 通常不需要手动调用——`start-my-day` 和 `paper-analyze` 会自动调用此 skill。

---

## 7. 进阶配置

### 7.1 全局 CLAUDE.md（自定义 AI 行为）

在 `~/.claude/CLAUDE.md` 中写入指令，对所有项目生效：

```markdown
# 我的 Claude Code 配置

## 基本规则
- 回答简洁直接，不要废话
- 代码修改前先读懂现有代码
- 优先修改现有文件，不随意新建文件

## Skill 调用规则
- 只在我明确要求时调用 Skills
- /start-my-day：我说"开始新的一天"时调用
```

### 7.2 项目级 CLAUDE.md

在项目根目录创建 `CLAUDE.md`，只对当前项目生效：

```markdown
# 项目说明

这是一个 FastAPI + PostgreSQL 后端项目。

## 技术栈
- Python 3.11
- FastAPI
- SQLAlchemy
- PostgreSQL

## 代码规范
- 使用 Black 格式化
- 所有函数必须有类型注解
- 测试覆盖率 > 80%
```

### 7.3 模型选择

```bash
# 在 Claude Code 中切换模型
/model

# 可选模型（截至 2025 年）：
# - claude-opus-4-6（最强，适合复杂任务）
# - claude-sonnet-4-6（均衡，推荐日常使用）
# - claude-haiku-4-5（最快，适合简单任务）
```

### 7.4 Memory 系统

Claude Code 支持持久化记忆，跨对话保留重要信息：

```
你：记住我用 Python 3.11，项目用 pytest 做测试

Claude：[保存到记忆系统，下次对话自动加载]
```

---

## 8. 常见问题

**Q: `claude` 命令找不到？**
```bash
# 检查 npm 全局安装路径是否在 PATH 中
npm bin -g
# 将该路径加入 PATH
```

**Q: API 请求报错 401？**
- 检查 `ANTHROPIC_API_KEY` 是否正确设置
- 确认 key 未过期，余额是否充足

**Q: `start-my-day` 报找不到配置文件？**
- 确认 `OBSIDIAN_VAULT_PATH` 已设置
- 确认 `research_interests.yaml` 路径正确：`$OBSIDIAN_VAULT_PATH/99_System/Config/research_interests.yaml`

**Q: Skills 命令不生效？**
- 确认 skill 目录在 `~/.claude/skills/<skill-name>/SKILL.md`
- 重启 Claude Code

**Q: Windows 下路径问题？**
- 推荐使用 Git Bash 或 WSL2
- 路径使用正斜杠：`D:/ObsidianVault`（而非 `D:\ObsidianVault`）

---

## 贡献

欢迎提 Issue 或 PR！如果你有自己的 Skills，也欢迎分享。

## License

MIT
