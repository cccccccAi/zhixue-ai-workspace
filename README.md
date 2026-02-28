# 智学网 AI 工作台

智学网产品团队的 AI 技能工具箱。两个即用技能，帮你用 AI 完成产品经理的核心工作。

## 这是什么

这不是一个需要学习的框架，也不是一套需要配置的工具链。它是一个打开就能用的 AI 工作台：

- **学习路径**：6 关闯关教学，从整理指标体系到创建团队专属 AI 技能
- **生产工具**：PRD 生成器，内置智学网的实体定义、指标字典和质检清单

基于 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 的 Skill 机制，所有知识和流程都编码在 markdown 文件中。

## 前提条件

1. 安装 Claude Code（[安装指南](https://docs.anthropic.com/en/docs/claude-code/getting-started)）
2. 确保有 Claude 账号（Max 订阅或 API Key）

## 快速开始

```bash
# 1. 克隆仓库
git clone https://github.com/cccccccAi/zhixue-ai-workspace.git

# 2. 进入目录
cd zhixue-ai-workspace

# 3. 启动 Claude Code
claude
```

启动后会自动显示可用技能。输入 `/learn-pm-data` 开始闯关，或输入 `/write-prd 简化教师报告` 直接生成 PRD。

## 技能介绍

### /learn-pm-data — AI 产品力训练营

6 关逐步解锁，每关约 15 分钟：

| 关卡  | 主题         | 产出                          |
| ----- | ------------ | ----------------------------- |
| 第1关 | 指标体系整理 | 指标框架文档 + 实战提示卡     |
| 第2关 | 用研综合实战 | 用研综合报告 + 实战提示卡     |
| 第3关 | 竞品分析     | 竞品分析简报 + 实战提示卡     |
| 第4关 | 数据叙事     | 数据叙事文档 + 实战提示卡     |
| 第5关 | 数据看板     | 交互式 Dashboard + 实战提示卡 |
| 第6关 | 创建AI技能   | 智学网定制数据分析技能        |

每关支持真实数据输入，也提供智学网模拟数据。随时可以中断，进度自动保存。

### /write-prd — PRD 生成器

5 步生成可交付评审的 PRD：

1. **需求锚定**：驱动力、时间约束、读者
2. **素材提取**：自动读取闯关训练产出
3. **骨架生成**：12 章节 PRD，真实数据用占位符标记
4. **数据填充**：引导逐项补充真实数据
5. **交付质检**：20 项检查，阻塞项必须修复

内置智学网领域知识：实体关系、数据表结构、指标精确定义、角色术语。

## 示例产出

`examples/` 目录包含示例产出供参考：

- `level-1-zhixue-metrics.md` — 第1关指标体系整理产出
- `prd-report-simplify.md` — PRD 示例（教师报告简化）

## 反馈

使用中遇到问题或有改进建议，欢迎在飞书群反馈：

[待填: 飞书群链接或群名称]

## 致谢

本项目的教学设计参考了 [Anthropic knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins) 的产品管理和数据分析插件。
