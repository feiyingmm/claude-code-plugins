# Claude Code Plugins

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Claude Code 插件集合，包含实用的 Skills 工具，帮助提升开发效率。

## 插件列表

| Skill | 描述 | 触发方式 |
|-------|------|----------|
| [github-doc-generator](#github-doc-generator) | 分析 GitHub Skills/Plugins 项目并生成中文使用教程文档 | 手动触发 |

---

## 安装方式

### Claude Code 安装

```bash
# 方法一：通过 URL 安装（推荐）
claude plugin install https://github.com/feiyingmm/claude-code-plugins.git

# 方法二：在 Claude Code 中使用斜杠命令
/plugin install https://github.com/feiyingmm/claude-code-plugins.git

# 方法三：手动安装
git clone https://github.com/feiyingmm/claude-code-plugins.git
claude plugin add ./claude-code-plugins
```

### OpenCode 安装

```bash
# 克隆到 OpenCode plugins 目录
git clone https://github.com/feiyingmm/claude-code-plugins.git ~/.opencode/plugins/claude-code-plugins
```

---

## 卸载方式

### Claude Code 卸载

```bash
claude plugin remove github-doc-generator

# 或在 Claude Code 中
/plugin remove github-doc-generator
```

### OpenCode 卸载

```bash
rm -rf ~/.opencode/plugins/claude-code-plugins
```

---

## Skills 详情

### github-doc-generator

分析 GitHub 上的 Claude Code Skills/Plugins 项目，自动生成结构化的中文使用教程文档。

#### 触发方式

**手动触发**

```
/github-doc-generator <github-url>
```

#### 使用场景

- 快速了解一个 Claude Code Skills/Plugins 项目的功能
- 为开源项目生成中文使用文档
- 分析插件结构和安装方式

#### 命令示例

```bash
# 基本用法
/github-doc-generator https://github.com/anthropics/claude-code

# 自然语言方式
"帮我分析这个 GitHub 项目生成文档：https://github.com/xxx/xxx"
"给这个 skills 项目写个使用教程"
"分析这个插件项目，生成中文文档"
```

#### 功能特性

| 特性 | 说明 |
|------|------|
| 代理支持 | 默认 `http://127.0.0.1:7897`，解决网络访问问题 |
| 混合获取策略 | 优先使用 GitHub API，必要时克隆深度分析 |
| 触发方式分析 | 自动识别每个 Skill 的触发方式（自动/手动/两者都支持） |
| 中文文档输出 | 生成完整的中文 Markdown 文档 |

#### 输出文档结构

生成的文档包含以下章节：

| 章节 | 内容 |
|------|------|
| 简介 | 项目描述和核心价值 |
| 功能特性 | 所有 Skills 功能摘要 |
| 环境要求 | Claude Code 版本、依赖等 |
| 安装方式 | Claude Code 和 OpenCode 安装步骤 |
| Skills 使用指南 | 每个 Skill 的触发方式、触发条件、使用场景、命令示例 |
| 配置选项 | 可配置项说明 |
| 常见问题 | FAQ |
| 相关链接 | GitHub 仓库链接 |

#### 代理配置

默认代理地址：`http://127.0.0.1:7897`

如需修改代理，可设置环境变量：

```bash
# Linux/macOS
export HTTPS_PROXY=http://127.0.0.1:7897
export HTTP_PROXY=http://127.0.0.1:7897

# Windows PowerShell
$env:HTTPS_PROXY = "http://127.0.0.1:7897"
$env:HTTP_PROXY = "http://127.0.0.1:7897"

# Windows CMD
set HTTPS_PROXY=http://127.0.0.1:7897
set HTTP_PROXY=http://127.0.0.1:7897
```

或配置 Git 代理：

```bash
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
```

#### 示例输出

**输入：**
```
/github-doc-generator https://github.com/anthropics/claude-code
```

**输出：**
当前目录生成 `claude-code.md` 文件，包含完整的中文使用教程。

---

## 项目结构

```
claude-code-plugins/
├── .claude-plugin/
│   └── plugin.json              # 插件元数据
├── skills/
│   └── github-doc-generator/
│       └── SKILL.md             # Skill 定义文件
├── LICENSE
├── README.md
└── .gitignore
```

---

## 贡献指南

欢迎贡献新的 Skills！

1. Fork 本仓库
2. 在 `skills/` 目录下创建新的 Skill 文件夹
3. 添加 `SKILL.md` 文件，包含 YAML frontmatter（name, description）
4. 提交 Pull Request

### Skill 文件模板

```markdown
---
name: your-skill-name
description: Skill 描述，说明功能和触发条件
---

# Skill 标题

[详细的 Skill 实现内容]
```

---

## 许可证

[MIT License](LICENSE)

---

## 反馈与支持

如有问题或建议，请提交 [Issue](https://github.com/feiyingmm/claude-code-plugins/issues)。