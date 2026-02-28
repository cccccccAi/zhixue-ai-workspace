# 第5关：数据价值看板

> 第4关教了数据叙事方法论，这一关把叙事变成交互式看板。

## 目标
真实生成一个交互式 HTML Dashboard，展示智学网大数据价值。可在浏览器直接打开。

## 时长
约 15 分钟

## 教学流程

### Step 1：场景设定 + 与数据叙事衔接

告诉用户：「上一关你学会了用数据讲故事。现在把故事变成一个可交互的看板——受众不只是看你写的结论，还能自己点击探索数据。」

关键教学点：「Dashboard 设计的第一步不是选图表，而是问：这个看板要帮受众做什么决策？（和上一关一样，受众决定一切。）」

### Step 1.5：真实数据入口

用 AskUserQuestion 问：「你有自己的数据想做成看板吗？」
- "我来粘贴我的真实数据（CSV/表格/数据列表）"（真实数据路线）
- "用智学网模拟数据"（模拟路线）

**真实数据路线**：请用户粘贴数据，AI 会将其转换为 Dashboard 使用的 JSON 格式。

### Step 2：确定受众

用 AskUserQuestion 问：「这个看板的主要受众是？」
- 教育局领导（需要政策层面的宏观数据：覆盖率、整体效果）
- 学校校长（需要校级对比：我校 vs 全区、各科表现）
- 产品团队内部（需要功能级数据：使用率、留存、漏斗）

### Step 3：选择 KPI

根据受众选择，用 AskUserQuestion 问：「看板顶部放 4 个 KPI 卡片，选哪些？」
- 选项 A（宏观）：覆盖学校数、活跃学生数、平均成绩提升、知识点诊断覆盖率
- 选项 B（效果）：学习时长、考试完成率、薄弱知识点减少数、教师使用效率提升
- 选项 C（自选）：从以下 8 个中选 4 个 [列出选项]

### Step 4：选择图表类型

用 AskUserQuestion 问：「展示成绩趋势最合适的图表是？」
- 折线图（展示 6 个月的趋势变化）（推荐）
- 柱状图（对比各学科的绝对值）
- 组合图（柱状表示考试量，折线表示提升率）

关键教学点：
- 折线图适合趋势
- 柱状图适合对比
- 组合图适合"量 + 率"双维度
- **标题要写洞察不写数据名**（第4关学到的原则），例如"数学进步幅度领先其他学科"而非"各学科平均分"
- KPI 卡片用 Green/Yellow/Red 状态色（第4关学到的状态信号）

### Step 5：生成 Dashboard

**这一步真实执行**，生成完整的 HTML 文件。

读取 `./data/skills/interactive-dashboard-builder/SKILL.md` 获取模板和最佳实践。

生成 `./learning/outputs/level-5-zhixue-dashboard.html`，包含：

**HTML 结构：**
- Dashboard 标题 + 副标题（受众相关）
- 4 个 KPI 卡片（带数值、环比变化、状态颜色）
- 主图表区：趋势图（用户选择的类型）
- 副图表区：学科对比柱状图 + 区域分布饼图
- 数据明细表（可排序）
- 筛选器：省份/地区下拉选择

**CSS 样式：**
- 专业配色（白底 + 蓝灰主色 + 绿/红状态色）
- 卡片阴影 + 圆角
- 响应式布局

**JavaScript：**
- Chart.js CDN 引入
- 数据嵌入为 JSON 变量
- 筛选器联动更新所有图表
- 表格列排序
- Hover 提示

**嵌入数据（JSON）：**
```javascript
const ZHIXUE_DATA = {
  monthly: [
    { month: '2025-09', schools: 2800, students: 4200000, assessments: 850000, avg_improvement: 3.2 },
    { month: '2025-10', schools: 2900, students: 4350000, assessments: 920000, avg_improvement: 4.1 },
    { month: '2025-11', schools: 2950, students: 4400000, assessments: 780000, avg_improvement: 4.5 },
    { month: '2025-12', schools: 3000, students: 4420000, assessments: 1200000, avg_improvement: 4.8 },
    { month: '2026-01', schools: 3050, students: 4480000, assessments: 1350000, avg_improvement: 5.0 },
    { month: '2026-02', schools: 3100, students: 4500000, assessments: 900000, avg_improvement: 5.2 }
  ],
  subjects: [
    { name: '语文', avg_score: 78.5, improvement: 3.8, completion: 0.92 },
    { name: '数学', avg_score: 72.3, improvement: 5.2, completion: 0.88 },
    { name: '英语', avg_score: 75.1, improvement: 4.1, completion: 0.90 },
    { name: '物理', avg_score: 68.7, improvement: 6.3, completion: 0.85 },
    { name: '化学', avg_score: 71.2, improvement: 4.8, completion: 0.87 },
    { name: '生物', avg_score: 76.8, improvement: 3.5, completion: 0.91 }
  ],
  regions: [
    { name: '安徽省', schools: 1200, students: 1500000, improvement: 5.5 },
    { name: '江苏省', schools: 800, students: 980000, improvement: 4.8 },
    { name: '浙江省', schools: 450, students: 620000, improvement: 5.1 },
    { name: '山东省', schools: 380, students: 850000, improvement: 4.2 },
    { name: '河南省', schools: 270, students: 550000, improvement: 4.9 }
  ],
  knowledge_gaps: [
    { topic: '函数与方程', weak_students: 45000, avg_score: 52 },
    { topic: '阅读理解推断', weak_students: 38000, avg_score: 55 },
    { topic: '电磁学基础', weak_students: 32000, avg_score: 48 },
    { topic: '完形填空', weak_students: 29000, avg_score: 51 },
    { topic: '化学平衡', weak_students: 26000, avg_score: 53 },
    { topic: '文言文翻译', weak_students: 24000, avg_score: 56 },
    { topic: '概率统计', weak_students: 22000, avg_score: 54 },
    { topic: '力学综合', weak_students: 20000, avg_score: 50 },
    { topic: '作文结构', weak_students: 18000, avg_score: 58 },
    { topic: '有机化学', weak_students: 15000, avg_score: 57 }
  ]
};
```

### Step 6：预览与讲解

生成文件后：
1. 用 `open ./learning/outputs/level-5-zhixue-dashboard.html` 在浏览器打开
2. 简要讲解：
   - KPI 卡片的设计要点（数值 + 环比 + 颜色 = 10 秒理解）
   - 图表标题要写洞察（"数学进步幅度最大"而非"各科平均分"）
   - 筛选器让受众自己探索
3. 用 AskUserQuestion 问：「你觉得这个看板还需要调整什么？」
   - 配色需要调整
   - 想加一个图表
   - 数据维度不够
   - 很好，不需要改

如果需要调整，做一次修改后再次打开预览。

### Step 7：实战提示卡

展示实战提示卡：
```
以后需要数据看板时，对 AI 说：
"帮我生成一个交互式 HTML Dashboard。
受众：[谁在看]
他们需要做的决策：[什么决策]
KPI（4个）：[指标1]、[指标2]、[指标3]、[指标4]
数据：[粘贴 CSV/表格数据]
要求：Chart.js 图表、筛选器联动、标题写洞察、
自包含 HTML 文件可直接浏览器打开。"
```

## 验证条件
- `./learning/outputs/level-5-zhixue-dashboard.html` 文件已生成
- 文件大小 > 5KB（确保是完整 HTML）
