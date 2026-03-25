# GitHub Doc Generator Plugin

分析 GitHub 上的 Claude Code Skills/Plugins 项目并生成中文使用教程文档。

## 功能特性

- 支持代理（默认 `http://127.0.0.1:7897`）
- 混合获取策略（GitHub API + 必要时克隆）
- 详细提取每个 Skill 的触发方式、触发条件、使用场景
- 生成包含 Claude Code 和 OpenCode 安装方式的文档

## 使用方法

```
/github-doc-generator <github-url>
```

**示例：**

```
/github-doc-generator https://github.com/anthropics/claude-code
```

或自然语言：

```
"帮我分析这个 GitHub 项目生成文档：https://github.com/xxx/xxx"
```

## 输出

生成的文档包含：
- 项目简介和核心价值
- 功能特性列表
- 环境要求
- Claude Code 和 OpenCode 安装方式
- Skills 使用指南（触发方式、触发条件、使用场景、命令示例）
- 配置选项
- 常见问题

## 许可证

MIT License