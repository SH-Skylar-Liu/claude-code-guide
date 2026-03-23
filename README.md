# 传媒人的 Vibe Coding 指南

> 用 Claude Code 做传媒人里少有的技术加成——视听素材处理、广告分析、可视化仪表盘、自动化流程

这份指南专为**传媒/视听/广告**背景同学准备，目标是用 AI 快速产出有行业价值的作品放进简历。

---

## 为什么传媒人要会 Vibe Coding？

能用 AI 工具处理视觉内容、分析广告数据、自动化重复工作的人是稀缺的。你不需要成为程序员，但你需要能：

- 批量处理几百张图片/视频素材，几分钟完成过去要手动做一天的活
- 用数据分析广告创意效果，提出有依据的优化建议
- 做出可以公开访问的交互仪表盘，而不只是截图
- 把重复工作变成自动运行的流程

这就是 **Vibe Coding**：你主导方向，AI 写代码，你理解和调整结果。

---

## 目录

1. [安装配置（15分钟）](#1-安装配置)
2. [Vibe Coding 基本用法](#2-vibe-coding-基本用法)
3. [Skills 完整指南](#3-skills-完整指南)
4. [模块一：视听素材批量处理](#4-模块一视听素材批量处理)
5. [模块二：广告内容分析](#5-模块二广告内容分析)
6. [模块三：Streamlit 交互仪表盘](#6-模块三streamlit-交互仪表盘)
7. [模块四：自动化 Pipeline](#7-模块四自动化-pipeline)
8. [模块五：让面试官眼前一亮](#8-模块五让面试官眼前一亮)
9. [番外：舆情与数据可视化](#9-番外舆情与数据可视化)
10. [简历作品集搭建](#10-简历作品集搭建)
11. [常见问题](#11-常见问题)

---

## 1. 安装配置

### Node.js（Claude Code 需要）

去 [nodejs.org](https://nodejs.org/) 下载 LTS 版，一路下一步。

```bash
node --version   # v18.x 或更高
```

### Python + 常用库

去 [python.org](https://www.python.org/downloads/) 下载 3.10+，安装时勾选 **Add Python to PATH**。

```bash
# 基础库
pip install pandas matplotlib seaborn plotly openpyxl

# 图像处理
pip install pillow opencv-python

# 视频处理
pip install moviepy

# 仪表盘
pip install streamlit

# 调用 AI API
pip install anthropic
```

> FFmpeg（视频处理必需）：去 [ffmpeg.org](https://ffmpeg.org/download.html) 下载，解压后把 `bin/` 目录加入系统 PATH。

### 获取并设置 API Key

1. 打开 [console.anthropic.com](https://console.anthropic.com/) 注册
2. **API Keys** → **Create Key** → 复制

```bash
# macOS/Linux
echo 'export ANTHROPIC_API_KEY="sk-ant-api03-你的key"' >> ~/.zshrc && source ~/.zshrc

# Windows PowerShell
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_API_KEY", "sk-ant-api03-你的key", "User")
```

### 安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
claude --version   # 验证安装
```

启动：
```bash
cd 你的项目文件夹
claude
```

---

## 2. Vibe Coding 基本用法

你不需要懂代码，但要学会**描述清楚**。

**描述模板：**
```
我有 [什么素材/数据]（格式：jpg/mp4/csv/xlsx）
我想得到 [什么结果]
要求：[具体细节，如尺寸、格式、样式、数量]
```

遇到报错，把错误信息完整复制给 Claude：
```
运行后报错：[粘贴报错内容]
帮我修复，解释是什么原因
```

---

## 3. Skills 完整指南

Skills 是写在 `~/.claude/skills/` 里的自定义指令包，用 `/skill名称` 触发，让 Claude 按照特定流程工作。本仓库包含 22 个 skills，分两大类。

### 3.1 项目开发工作流 Skills（让 Claude 更聪明地写代码）

这些 skills 的作用是**强迫 Claude 在动手之前先想清楚**，避免反复返工。对 vibe coding 新手来说特别有用。

| Skill | 触发方式 | 什么时候用 |
|-------|---------|-----------|
| `using-superpowers` | 说"调用superpowers" | 开启完整结构化开发模式，自动链式调用下面的子 skills |
| `brainstorming` | 自动（做任何新功能前） | Claude 先问清楚你想要什么，再动手，避免方向跑偏 |
| `writing-plans` | 自动（复杂任务） | 把大任务拆成步骤，执行前先对齐 |
| `test-driven-development` | 自动（写代码前） | 先写测试，再写实现，确保代码真的能跑 |
| `systematic-debugging` | 遇到 bug 时 | 系统性找原因，不随机乱试 |
| `verification-before-completion` | 说"完成"之前 | 验证代码真的能跑，不靠感觉说 done |
| `executing-plans` | 有计划要执行时 | 按步骤执行，有子代理把关 |
| `dispatching-parallel-agents` | 多个独立任务时 | 同时派多个 AI 并行处理，速度 N 倍提升 |
| `planning-with-files-zh` | 说"文件规划" | 创建持久化任务文件，跨对话保留进度 |
| `using-git-worktrees` | 开始新功能时 | 每个功能用独立工作区，互不干扰 |
| `finishing-a-development-branch` | 功能做完后 | 收尾、清理、准备合并 |
| `requesting-code-review` | 提交前 | 让 Claude 审查自己写的代码 |
| `receiving-code-review` | 收到审查意见后 | 处理审查反馈，不盲目执行 |
| `simplify` | 代码写完后 | 检查并简化代码 |

**实际使用示例：**

```
# 开始一个新的仪表盘项目前，说：
调用superpowers

# Claude 会自动：
# 1. 先用 brainstorming 问清楚你的需求
# 2. 用 writing-plans 拆解任务步骤
# 3. 执行时用 TDD 确保代码可靠
# 4. 完成前用 verification 验证结果
```

### 3.2 论文阅读工作流 Skills（学术/研究用）

这5个 skills 面向需要跟踪 AI 领域最新论文的用户，依赖 Obsidian vault。

| Skill | 触发方式 | 功能 |
|-------|---------|------|
| `start-my-day` | `/start-my-day [日期]` | 每日 arXiv 论文推荐，前3篇自动深度分析 |
| `paper-analyze` | `/paper-analyze [arXiv ID]` | 单篇论文深度分析，图文并茂生成笔记 |
| `paper-search` | `/paper-search [关键词]` | 在已有论文笔记中搜索 |
| `conf-papers` | `/conf-papers [年份] [会议]` | 搜索 CVPR/ICLR/NeurIPS 等顶会高引论文 |
| `extract-paper-images` | `/extract-paper-images [ID]` | 从 arXiv 源码包提取论文高质量图片 |

> 论文 skills 需要额外配置：设置 `OBSIDIAN_VAULT_PATH` 环境变量，详见 [skills/start-my-day/SKILL.md](skills/start-my-day/SKILL.md)

### 3.3 Skills 安装方式

```bash
# 克隆本仓库
git clone https://github.com/SH-Skylar-Liu/claude-code-guide.git

# 复制 skills 到 Claude Code 配置目录
cp -r claude-code-guide/skills/* ~/.claude/skills/

# 验证安装（启动 Claude Code 后）
# 输入 /brainstorming 或 说"调用superpowers"，看看有没有响应
```

---

## 4. 模块一：视听素材批量处理

### 3.1 图片批量处理

**按平台规格批量裁切：**
```
我有一个文件夹 /raw_photos/ 里有200张横版图片，
帮我批量处理成以下几个规格并分别保存到子文件夹：
- 微信公众号封面：900×383px
- 小红书封面：1242×1660px（竖版，居中裁切）
- 抖音封面：1080×1920px（竖版，居中裁切）
- 微博配图：1200×900px
原图保留不动
```

**批量加水印：**
```
给 /photos/ 里所有 jpg 文件加上水印：
- 水印文字："© 品牌名 2024"
- 位置：右下角，留10px边距
- 字体白色，半透明（透明度70%）
- 字号根据图片大小自适应（图片宽度的3%）
保存到 /output/ 文件夹，文件名加 _watermark 后缀
```

**批量格式转换和压缩：**
```
把 /images/ 里所有 PNG 转成 JPG，
质量压缩到85%，文件大小控制在500KB以内，
如果原图已经小于500KB就保持原质量
统计转换前后的总文件大小，告诉我节省了多少空间
```

**提取图片主色调（用于品牌分析）：**
```
读取 /ad_images/ 里的所有图片，
提取每张图的 Top5 主色调（HEX值和占比），
输出到 color_palette.xlsx，
并画一张可视化图：每张广告图对应一排色块
```

### 3.2 视频批量处理

**批量提取封面帧：**
```
/videos/ 里有30个 mp4 文件，
帮我从每个视频中提取3帧：第1秒、中间时间点、最后3秒前，
保存为 jpg，文件名格式：视频名_frame1.jpg
```

**批量剪切片段：**
```
videos.xlsx 里记录了视频文件名、开始时间、结束时间，
帮我用 FFmpeg 批量截取这些片段，
保存到 /clips/ 文件夹，保持原始画质
```

**视频转 GIF（社媒用）：**
```
把 demo.mp4 的第5秒到第12秒转成 GIF，
宽度640px，帧率15fps，
同时输出一个画质更好的 WebP 版本
```

**批量添加字幕条：**
```
给 /videos/ 里的视频批量加底部字幕条：
- 黑色半透明背景条，高度占视频高度的12%
- 从 subtitles.csv 读取对应的字幕文字（按文件名匹配）
- 白色字体，居中
用 FFmpeg 实现，保持原视频分辨率
```

### 3.3 素材管理自动化

**自动整理文件夹：**
```
扫描 /downloads/ 文件夹，
按文件类型自动分类到子文件夹：
- 图片（jpg/png/webp/gif）→ /images/
- 视频（mp4/mov/avi）→ /videos/
- 文档（pdf/docx/xlsx）→ /docs/
按修改日期重命名：YYYYMMDD_原文件名
生成一个整理报告 report.txt，说明移动了多少文件
```

---

## 5. 模块二：广告内容分析

### 4.1 用 AI 批量分析广告创意

这是最有差异化的能力：用 Claude 的视觉理解能力自动分析广告图片。

**批量分析广告图：**
```python
# 让 Claude Code 帮你写这个脚本

# 目标：读取 /ad_images/ 里的广告图，
# 用 Claude API 分析每张图，输出：
# - 广告类型（产品展示/生活方式/促销/品牌形象）
# - 主视觉元素（人物/产品/场景/文字占比）
# - 色调风格（暖色/冷色/高饱和/低饱和/黑白）
# - 文案密度（文字多/适中/极少）
# - 情感基调（活力/专业/温馨/奢华/亲民）
# 结果保存到 ad_analysis.xlsx
```

对 Claude Code 说：
```
帮我写一个脚本，读取文件夹里的广告图片，
调用 Claude API（claude-opus-4-5 模型，支持图片输入），
用这个提示词分析每张图：
"分析这张广告图，用JSON格式返回：
ad_type（广告类型）、visual_elements（主要视觉元素列表）、
color_tone（色调描述）、copy_density（文案密度：多/中/少）、
emotional_tone（情感基调）、target_audience（推测目标人群）"
把所有结果汇总到 Excel
```

### 4.2 广告数据效果分析

**投放数据分析：**
```
我有广告投放数据 ad_performance.xlsx，
字段有：广告ID、素材类型、投放平台、花费、曝光量、点击量、转化量、投放日期

帮我分析：
1. 各素材类型的 CTR（点击率）和 CVR（转化率）对比
2. 各平台的 ROI 对比（用转化量/花费）
3. 投放效果随时间的变化趋势
4. 找出 CTR 最高的 Top10 素材，提取它们的共同特征

画4张图，风格简洁专业，适合放进汇报 PPT
```

**竞品广告监测分析：**
```
competitor_ads.xlsx 记录了竞品的广告投放信息：
品牌名、广告文案、投放平台、预估曝光、发布日期

帮我分析：
1. 各品牌的投放频率和平台偏好
2. 文案关键词词频对比（按品牌分组的词云）
3. 活动节点投放强度（节假日前后的投放量变化）
```

### 4.3 素材效果关联分析

把创意分析和投放数据结合起来，这是真正的差异化：

```
我有两个文件：
- ad_analysis.xlsx：每个广告素材的创意特征（颜色/元素/风格）
- ad_performance.xlsx：对应的投放效果数据

帮我把两个文件按广告ID合并，然后分析：
1. 哪种色调风格的 CTR 更高？
2. 文案密度和转化率有关系吗？
3. 不同情感基调在各平台的表现差异

画一个热力图：行=创意特征，列=效果指标，值=平均分
```

---

## 6. 模块三：Streamlit 交互仪表盘

Streamlit 可以把分析结果做成可公开访问的网页——面试直接发链接，比截图专业10倍。

### 5.1 快速上手

```
帮我用 Streamlit 写一个广告效果分析仪表盘，
读取 ad_performance.xlsx，包含以下功能：
- 侧边栏：可以按平台、时间范围筛选数据
- 顶部4个指标卡：总花费、总曝光、平均CTR、总转化
- 折线图：所选时间范围内的每日曝光趋势
- 柱状图：各素材类型的CTR对比
- 数据表：可排序的明细表

运行命令：streamlit run dashboard.py
```

### 5.2 图片/视频内容仪表盘

```
帮我做一个素材库管理仪表盘（Streamlit），功能：
- 上传图片 → 自动显示尺寸、文件大小、主色调
- 侧边栏按类型/日期筛选
- 图片网格展示（每行4张，点击放大）
- 导出选中素材的清单到 Excel

界面要好看，用深色主题
```

### 5.3 广告创意分析仪表盘（最适合放简历）

```
做一个广告创意分析仪表盘：
- 左侧上传广告图片
- 右侧实时显示 AI 分析结果：
  创意类型、视觉元素、色调、情感基调、改进建议
- 底部显示历史分析记录（表格）
- 支持批量上传，批量分析完后可导出 Excel 报告

用 Streamlit + Claude API 实现
```

### 5.4 部署上线（免费）

代码写好后，用 Streamlit Cloud 免费部署：

```bash
# 1. 把项目推到 GitHub（参考本仓库的提交方式）

# 2. 去 share.streamlit.io 用 GitHub 登录

# 3. 选择你的仓库和主文件，点 Deploy

# 部署完会得到一个公开链接，比如：
# https://yourname-ad-dashboard-app-xxxxx.streamlit.app
```

> 整个过程约5分钟，完全免费，链接可以直接发给面试官。

---

## 7. 模块四：自动化 Pipeline

把重复工作变成定时自动运行的流程，这是内容运营/数据岗非常看重的能力。

### 6.1 素材处理自动化

**监控文件夹，自动处理新素材：**
```
帮我写一个脚本，用 watchdog 库监控 /inbox/ 文件夹：
- 有新图片进来 → 自动裁成3个平台尺寸，移到 /processed/
- 有新视频进来 → 自动提取第1秒的封面帧，移到 /thumbnails/
- 处理完在 log.txt 里记录时间和文件名

脚本在后台持续运行
```

### 6.2 数据自动更新

**每天自动拉数据更新报告：**
```
帮我写一个定时脚本（用 schedule 库，每天早上9点运行）：
1. 从 Google Sheets 读取最新广告数据（用 gspread 库）
2. 更新本地 ad_performance.xlsx
3. 重新生成昨日效果报告图（4张图）
4. 把报告图打包发邮件给我（用 smtplib）

把敏感信息（邮箱密码等）放在 .env 文件里，不要写死在代码里
```

### 6.3 完整的内容分析 Pipeline

```
帮我搭一个完整 pipeline，按顺序执行：

第1步：从指定文件夹读取本周新增的广告素材（图片）
第2步：用 Claude API 批量分析每张图的创意特征
第3步：从 ad_performance.xlsx 读取对应的投放效果数据
第4步：合并创意特征和投放数据，计算关联指标
第5步：更新 Streamlit 仪表盘的数据文件
第6步：生成本周分析报告（Word文档）
第7步：发邮件通知处理完成

每步完成后打印进度日志，出错不中断，记录到 error.log
```

### 6.4 Pipeline 项目结构

```
ad_analysis_pipeline/
├── data/
│   ├── raw/              # 原始素材
│   ├── processed/        # 处理后的素材
│   └── reports/          # 输出报告
├── scripts/
│   ├── 01_process_assets.py    # 素材处理
│   ├── 02_analyze_creatives.py # AI分析创意
│   ├── 03_merge_performance.py # 合并效果数据
│   └── 04_generate_report.py   # 生成报告
├── dashboard/
│   └── app.py            # Streamlit仪表盘
├── run_pipeline.py       # 一键运行全流程
├── .env                  # API Keys（不提交到Git）
└── requirements.txt      # 依赖列表
```

---

## 8. 模块五：让面试官眼前一亮

以下几项是大多数候选人做不到的，但用 Claude Code vibe coding 可以快速实现。

### 8.1 GitHub Actions：Pipeline 跑在云端

本地 cron 脚本有个致命问题——电脑关机就停了。GitHub Actions 让你的 pipeline **跑在云端，每天自动执行，完全免费**。面试时说"我的分析系统每天自动运行并发报告"，比"我写了个脚本"有力得多。

```yaml
# 在项目里创建 .github/workflows/daily_report.yml
# 让 Claude Code 帮你写这个文件：
```

对 Claude 说：
```
帮我创建 GitHub Actions workflow 文件：
- 每天北京时间早上9点自动运行
- 步骤：
  1. 安装 Python 依赖（requirements.txt）
  2. 运行 scripts/fetch_data.py 拉取最新广告数据
  3. 运行 scripts/analyze.py 生成分析报告
  4. 把生成的图表文件提交到仓库的 output/ 目录
- API Key 从 GitHub Secrets 读取，不要写死在代码里
```

生成的 workflow 推到 GitHub 后，Actions 自动每天执行，仓库里的报告自动更新。

### 8.2 Gradio：30 秒做出可分享的 AI Demo

Streamlit 适合数据仪表盘，Gradio 更适合**AI 功能演示**——上传一张广告图，立刻看到 AI 分析结果。部署到 Hugging Face Spaces，完全免费，有公开链接。

```
帮我用 Gradio 做一个广告创意分析 Demo：
- 界面：左边上传图片，右边显示分析结果
- 分析内容：调用 Claude API，分析广告的
  创意类型、目标受众、情感基调、改进建议
- 底部加一个"批量分析"Tab：上传多张图，
  输出汇总表格并可以下载 Excel
- 风格简洁，适合演示用

部署命令：直接推到 Hugging Face Spaces
```

部署到 Hugging Face Spaces：

```bash
# 安装 huggingface_hub
pip install huggingface_hub

# 登录（去 huggingface.co 注册后获取 token）
huggingface-cli login

# 创建 Space 并推送
# 去 huggingface.co/new-space 创建，选 Gradio 类型
# 然后像推 GitHub 一样 git push
```

最终得到类似 `https://huggingface.co/spaces/你的名字/ad-analyzer` 的公开链接。

### 8.3 多智能体并行处理：10 倍速分析

普通方式分析100张广告图：逐张调用 API，等待，慢。

用 `dispatching-parallel-agents` skill，Claude 会同时派出多个子代理并行处理——速度提升 N 倍，这是展示"懂 AI 工程"的好机会。

对 Claude Code 说：
```
我有100张广告图需要用 Claude API 分析，
帮我用多线程并行处理，同时运行10个线程，
每个线程处理10张，结果合并到一个 Excel。
加上进度条（tqdm），出错自动重试3次。
```

### 8.4 实时数据接入仪表盘

把 Streamlit 仪表盘连接到真实数据源，而不是本地 Excel 文件：

```
帮我把 Streamlit 仪表盘改造成连接 Google Sheets 的实时版本：
- 用 gspread 库读取 Google Sheets 数据
- 每5分钟自动刷新（不用手动刷新页面）
- 加一个"上次更新时间"显示
- 支持在仪表盘里直接编辑数据并写回 Sheets
```

配合 Google Sheets 做数据源，团队任何人更新表格，仪表盘自动更新——这是真实工作场景里最有价值的能力。

### 8.5 视频广告 AI 逐帧分析

这是传媒岗几乎没人能做到的：

```
帮我写一个视频广告分析脚本：
1. 用 FFmpeg 每秒提取1帧，共提取前30秒
2. 把每帧图片用 Claude Vision API 分析：
   画面内容、情绪基调、产品出现时间点
3. 汇总成时间轴报告：
   0-5秒：场景建立（xx画面）
   5-15秒：产品展示（xx特点）
   15-30秒：行动号召（xx文案）
4. 生成可视化时间轴图表
```

这份"视频广告创意解构报告"作为作品展示，在广告/品牌岗面试里会非常突出。

---

## 9. 番外：舆情与数据可视化

### 社媒评论分析

```
帮我分析 weibo_comments.csv 的用户评论：
1. 情感倾向分布（正面/负面/中性）饼图
2. 高频词柱状图（Top20，过滤停用词）
3. 评论量按天的时间趋势
配色参考彭博社的新闻图表风格
```

### 公开数据可视化

```
我有国家统计局的数据 stats.xlsx，
帮我用 plotly 做交互式图表：
- 鼠标悬停显示数值
- 支持时间范围缩放
- 保存为 HTML，可直接在浏览器打开
```

---

## 10. 简历作品集搭建

### 推荐的 3 个项目

**项目一：广告创意分析仪表盘**（最有说服力）
- 上传广告图 → AI自动分析创意特征 → 关联投放效果 → 可视化展示
- 部署到 Streamlit Cloud，有公开访问链接
- 亮点：技术 + 广告洞察结合，传媒科班做不到

**项目二：素材批量处理工具**
- 支持图片/视频按平台规格批量处理
- 有简单的命令行界面或 Streamlit 界面
- 亮点：解决真实工作痛点，展示工程思维

**项目三：内容效果自动化报告**
- 每周自动拉数据→分析→发邮件报告
- 展示完整 pipeline
- 亮点：展示可以省人力的自动化能力

### 怎么展示

1. **GitHub 仓库**：代码 + README 说明项目背景和结论
2. **Streamlit 公开链接**：面试直接发，现场演示
3. **简历写法**："独立搭建广告创意分析系统，用 AI 自动分析500+素材创意特征，发现高CTR素材的视觉规律"

---

## 11. 常见问题

**Q: FFmpeg 装了但找不到命令？**
```bash
# 检查是否在 PATH 里
ffmpeg -version
# 如果报错，把 FFmpeg 的 bin/ 目录手动加入系统环境变量 PATH
```

**Q: Claude API 分析图片怎么传图？**
```
帮我写一个函数，读取本地图片文件，
用 base64 编码后调用 Claude API（claude-opus-4-5），
传入图片和文字提示词，返回分析结果
```

**Q: Streamlit 仪表盘本地跑得好，部署后报错？**
```
我的 Streamlit 应用本地运行正常，
部署到 Streamlit Cloud 后报错：[粘贴报错]
帮我排查，注意云端没有本地文件系统权限
```

**Q: 视频处理很慢怎么办？**
```
视频批量处理太慢了，帮我用多进程（multiprocessing）
并行处理，同时处理4个视频，注意别超过CPU核心数
```

**Q: API 余额不够？**
- 分析图片用 `claude-haiku-4-5` 模型，比 Sonnet 便宜很多，速度也快
- 批量任务加 `time.sleep(0.5)` 避免被限速

---

> 问题或改进建议：[提 Issue](https://github.com/SH-Skylar-Liu/claude-code-guide/issues)
