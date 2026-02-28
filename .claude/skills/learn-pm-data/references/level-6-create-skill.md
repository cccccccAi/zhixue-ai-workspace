# 第6关：创建智学网专属技能 — 毕业

## 目标
用 data-context-extractor 元技能模式，真实创建一个智学网数据分析 Skill，可以部署给团队使用。

## 时长
约 15 分钟

## 教学流程

### Step 1：元技能概念

告诉用户：「前 5 关你用了别人做好的技能。现在你要自己创造一个。」

介绍 data-context-extractor 的独特之处：
- 普通 skill：做某件具体的事（写 PRD、做竞品分析）
- 元技能（meta-skill）：**创造新的 skill**
- 类比：普通 skill 是菜谱，元技能是"教你写菜谱的方法论"

### Step 2：展示案例

读取 `./data/skills/data-context-extractor/references/example-output.md`，展示 ShopCo 案例的结构（不展示全文，概括关键部分）：

- SKILL.md：包含实体说明、术语表、标准过滤条件、指标定义
- references/：按业务领域拆分的详细参考文件

关键教学点：「这个 skill 的目标是：让任何一个新来的分析师，读完这个 skill 就能正确查询你们公司的数据，不犯常见错误。」

### Step 3：实体消歧

用 AskUserQuestion 问：「在智学网的数据里，当有人说"用户"时，可能指？」（多选）
- 学生（Student）
- 教师（Teacher）
- 学校管理员（School Admin）
- 教育局人员（Education Bureau）
- 家长（Parent）

关键教学点：「实体消歧是数据 skill 的第一步。同一个词在不同人嘴里含义不同，这是数据查询出错的头号原因。」

### Step 4：定义核心指标

用 AskUserQuestion 问：「团队最常问的 2-3 个数据问题是？」（多选）
- 考试完成率（assessment completion rate）
- 知识点掌握率（knowledge mastery rate）
- 学习进步幅度（learning improvement score）
- 教师工具使用率（teacher tool adoption）
- 学校数据报告覆盖率（school report coverage）

对每个选中的指标，AI 追问精确定义（用 AskUserQuestion）：
- 分子是什么？分母是什么？
- 时间粒度？（日/周/月/学期）
- 需要排除什么？（模考、测试数据）

### Step 5：共创 Description

告诉用户：「现在最关键的一步——你来写这个 skill 的 description。1-2 句话，说清楚这个技能是做什么的、给谁用的。」

提供 3 种方式（AskUserQuestion）：
- "我自己写"（用户直接输入）
- "给我一个草稿，我来改"（AI 生成草稿，用户修改）
- "帮我写，我来确认"（AI 生成，用户确认或调整）

无论哪种方式，最终 description 必须由用户确认。

### Step 6：常见数据错误

用 AskUserQuestion 问：「在智学网数据查询中，最常见的错误是？」
- 混淆不同学期的考试成绩（跨学期对比不排除年级变化）
- 不过滤模拟考/练习考的数据（把模考混入正式考）
- 混淆累计学生数和当期活跃学生数
- 不区分不同考试类型的权重（月考 vs 期中 vs 期末）

### Step 7：生成 Skill

AI 创建完整的 skill 目录：

```
./learning/outputs/level-6-zhixue-data-analyst/
├── SKILL.md
└── references/
    ├── entities.md
    ├── metrics.md
    └── assessments.md
```

**SKILL.md 内容结构：**
```yaml
---
name: zhixue-data-analyst
description: [用户写的 description]
---
```

正文包含：
1. 实体说明（学生、教师、学校、考试的关系和消歧）
2. 术语表（智学网特有术语）
3. 标准过滤条件（排除测试数据的 SQL 示例）
4. 核心指标速查（指标名、定义、计算公式）
5. 常见错误（用户选择的 + 额外补充）
6. 参考文件导航（指向 references/ 下各文件）

**references/entities.md**：实体关系说明（学生-班级-学校-区域层级、教师-学科-年级关系）

**references/metrics.md**：每个核心指标的详细定义（含 SQL 伪代码示例）

**references/assessments.md**：考试数据领域说明（考试类型、成绩维度、知识点映射）

### Step 8：审查与修改

展示生成的 SKILL.md 核心内容（不超过 30 行），用 AskUserQuestion 问：
「你觉得需要修改哪个部分？」
- 修改 description
- 修改某个指标定义
- 添加一个常见错误
- 看起来不错，不改了

如果需要修改，执行一次修改后完成。

### Step 9：部署指引

告诉用户下一步怎么用：
1. 把 `level-6-zhixue-data-analyst/` 复制到 `.claude/skills/`
2. 团队成员启动 Claude Code 后，AI 会自动加载这个 skill
3. 任何人问数据问题时，AI 会用你定义的实体、指标、过滤规则来回答

**如何迭代**：
- Skill 不是一次性的，需要持续更新
- 新发现常见错误？→ 加到 SKILL.md 的"常见错误"部分
- 指标定义变了？→ 更新 references/metrics.md
- 新增了数据实体？→ 更新 references/entities.md
- 团队反馈查数据不准？→ 检查实体消歧和过滤条件

### Step 10：实战提示卡

展示实战提示卡：
```
以后需要创建新 Skill 时：
1. 确定技能边界：这个 skill 帮谁、做什么、什么场景下触发
2. 写 description（1-2 句话，给 AI 看的能力说明）
3. 列出实体消歧（术语表：同一个词可能指什么）
4. 定义核心指标/规则（精确的分子分母、时间粒度）
5. 列出常见错误（anti-patterns：团队踩过的坑）
6. 参考模板：./data/skills/data-context-extractor/references/skill-template.md

迭代方式：
- 让团队成员试用 → 收集"查数据不准"的案例 → 补充到 Skill 中
- 每季度审视一次，删除过时内容、补充新指标
```

## 验证条件
- `./learning/outputs/level-6-zhixue-data-analyst/SKILL.md` 文件存在
- frontmatter 包含 name（zhixue-data-analyst）和 description（非空）
- `references/` 目录下有至少 2 个 .md 文件
