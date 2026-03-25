---
name: github-doc-generator
description: 分析 GitHub 上的 Claude Code Skills/Plugins 项目并生成中文使用教程文档。当用户提供 GitHub 链接并要求生成文档、使用教程、安装指南时触发此 skill。支持自动分析项目结构、提取 skills 信息、生成详细的中文 Markdown 文档。
---

# GitHub Skills 文档生成器

分析 GitHub 上的 Claude Code Skills/Plugins 项目，生成结构化的中文使用教程文档。

## 触发条件

用户调用命令：`/github-doc-generator <github-url>`

或用户表达类似意图：
- "帮我分析这个 GitHub 项目生成文档"
- "给这个 skills 项目写个使用教程"
- "分析这个插件项目，生成中文文档"

## 执行流程

```
用户输入 GitHub URL
       ↓
验证 URL 有效性
       ↓
配置代理（默认 http://127.0.0.1:7897）
       ↓
[阶段1] GitHub API 获取关键文件
  - README.md
  - skills 目录结构
  - SKILL.md 或 skill 定义文件
  - package.json / settings.json 等配置
       ↓
超时处理 → 提示检查代理或网络
       ↓
提取项目名 → 作为输出文件名
       ↓
判断是否需要克隆深度分析
       ↓
[阶段2] 分析 Skills 内容
  - 识别所有 skills
  - 提取名称、描述、触发条件
  - 分析安装方式
       ↓
[阶段3] 生成文档
  - 按标准章节组织
  - 输出到当前目录：<project-name>.md
```

## 代理配置

默认代理：`http://127.0.0.1:7897`

使用 gh 命令时配置代理：
```bash
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
```

GitHub API 请求时添加代理环境变量：
```bash
export HTTPS_PROXY=http://127.0.0.1:7897
export HTTP_PROXY=http://127.0.0.1:7897
```

如果请求超时，提示用户：
> 连接 GitHub 超时，请检查代理设置（默认 http://127.0.0.1:7897）或网络连接

## 信息获取策略

### 轻量模式（优先）

通过 GitHub API 获取：
1. 仓库基本信息（名称、描述、语言）
2. README.md 内容
3. 目录结构（特别是 skills/、plugins/ 目录）
4. 关键配置文件（package.json, settings.json, CLAUDE.md 等）

使用 `gh api` 或直接访问 raw 文件：
```bash
gh api repos/{owner}/{repo}/contents/
gh api repos/{owner}/{repo}/readme
```

### 深度模式（必要时）

如果轻量模式信息不足：
1. 克隆仓库到临时目录
2. 遍历 skills 目录，读取所有 SKILL.md
3. 分析代码结构和依赖

## Skills 信息提取

对于每个 skill，提取以下信息：

| 信息项 | 来源 |
|--------|------|
| Skill 名称 | SKILL.md 的 `name` 字段 |
| 功能描述 | SKILL.md 的 `description` 字段 |
| 触发方式 | 分析内容判断：自动触发、手动命令、两者都支持 |
| 触发条件 | 查找 TRIGGER、触发条件、when to use 等关键词 |
| 使用命令 | 用户可调用的命令名 |
| 参数说明 | skill 中定义的参数 |

### 触发方式判断规则

**自动触发**：
- 包含 "TRIGGER when"、"自动触发"、"当用户..." 等描述
- 没有明确的用户调用命令

**手动触发**：
- 提供明确的调用命令如 `/skill-name`
- 需要用户主动执行

**两者都支持**：
- 同时具备自动触发条件和手动调用命令

## 输出文档结构

生成的 Markdown 文档必须遵循以下结构：

```markdown
# <项目名>

## 简介

[项目描述，包含核心价值和主要用途]

## 功能特性

[列出该项目包含的所有 skills 及其功能摘要]

- **<Skill 名称>**：<一句话描述功能>
- **<Skill 名称>**：<一句话描述功能>

## 环境要求

- Claude Code 版本：[如适用]
- 其他依赖：[如有]

## 安装方式

### Claude Code 安装

[具体安装步骤，例如：]

```bash
# 方法一：直接安装
claude skills install <github-url>

# 方法二：手动安装
git clone <github-url>
claude skills add ./<project-name>
```

### OpenCode 安装

[具体安装步骤，例如：]

```bash
# 安装到 OpenCode skills 目录
git clone <github-url> ~/.opencode/skills/<project-name>
```

## Skills 使用指南

### <Skill 名称 1>

**触发方式：** 自动触发 / 手动触发 `/命令名` / 两者都支持

**触发条件：** [自动触发时，描述什么场景下会自动触发此 skill]

**使用场景：** [描述该 skill 适用的具体场景和问题]

**命令示例：**

```
/命令名 [参数]
```

**示例说明：** [具体使用案例和预期效果]

---

### <Skill 名称 2>

[同上结构]

---

## 配置选项

[如有配置项，列出说明]

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| xxx | xxx | xxx |

## 常见问题

[如有 FAQ，列出]

## 相关链接

- GitHub 仓库：[原始链接]
- 文档生成时间：[当前日期]
```

## 错误处理

1. **URL 无效**：提示用户检查链接格式
2. **仓库不存在或私有**：提示用户确认仓库是否公开
3. **网络超时**：提示检查代理设置
4. **信息不足**：使用深度模式克隆分析

## 示例

**输入：**
```
/github-doc-generator https://github.com/example/claude-skills
```

**输出：**
当前目录生成 `claude-skills.md` 文件，包含完整的中文使用教程。