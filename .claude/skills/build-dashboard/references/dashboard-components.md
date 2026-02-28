# 看板组件库

> 本文件定义看板可用的 UI 组件和 Chart.js 代码模式。
> AI 生成看板时从此库选择组件组合。

## 技术基础

```html
<!-- Chart.js CDN（固定版本 + SRI） -->
<script
  src="https://cdn.jsdelivr.net/npm/chart.js@4.5.1"
  integrity="sha384-jb8JQMbMoBUzgWatfe6COACi2ljcDdZQ2OxczGA3bGNeWe+6DChMTBJemed7ZnvJ"
  crossorigin="anonymous"
></script>
```

## 配色方案

```javascript
const COLORS = {
  primary: "#1890FF", // 智学网品牌蓝
  success: "#52C41A", // 上升/达标
  warning: "#FAAD14", // 关注
  danger: "#FF4D4F", // 异常/下降
  neutral: "#8C8C8C", // 中性
  bg: "#F5F5F5", // 背景
  chart: ["#1890FF", "#52C41A", "#FAAD14", "#FF4D4F", "#722ED1", "#13C2C2"],
};
```

## KPI 卡片

```html
<div class="kpi-card">
  <div class="kpi-label">指标名称</div>
  <div class="kpi-value">78.5</div>
  <div class="kpi-change positive">↑ 5.2%</div>
  <div class="kpi-sub">较上期</div>
</div>
```

```css
.kpi-card {
  background: white;
  border-radius: 8px;
  padding: 20px;
  text-align: center;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  flex: 1;
  min-width: 160px;
}
.kpi-value {
  font-size: 32px;
  font-weight: bold;
  color: #1890ff;
}
.kpi-change {
  font-size: 14px;
  margin-top: 4px;
}
.kpi-change.positive {
  color: #52c41a;
}
.kpi-change.negative {
  color: #ff4d4f;
}
.kpi-label {
  font-size: 14px;
  color: #8c8c8c;
  margin-bottom: 8px;
}
```

## 筛选器

```html
<div class="filter-bar">
  <select id="filter-subject" onchange="dashboard.applyFilters()">
    <option value="all">全部学科</option>
    <option value="数学">数学</option>
    <option value="语文">语文</option>
    <option value="英语">英语</option>
  </select>
  <select id="filter-grade" onchange="dashboard.applyFilters()">
    <option value="all">全部年级</option>
  </select>
</div>
```

## 图表类型选择指南

| 目的     | 图表类型    | Chart.js type      | 适用场景                 |
| -------- | ----------- | ------------------ | ------------------------ |
| 趋势对比 | 折线图      | 'line'             | 成绩趋势、使用率变化     |
| 分类对比 | 柱状图      | 'bar'              | 各班均分、各学科对比     |
| 构成占比 | 饼图/环形图 | 'doughnut'         | 分数段分布、功能使用占比 |
| 排名     | 横向柱状图  | 'bar' (horizontal) | 学校排名、知识点正确率   |
| 分布     | 柱状图      | 'bar'              | 分数段分布               |
| 矩阵     | 自定义 HTML | table + 颜色       | 知识点掌握热力图         |

## 折线图代码模式

```javascript
new Chart(ctx, {
  type: "line",
  data: {
    labels: ["第1次", "第2次", "第3次", "第4次"],
    datasets: [
      {
        label: "全班均分",
        data: [72, 75, 71, 78],
        borderColor: COLORS.primary,
        backgroundColor: COLORS.primary + "20",
        fill: true,
        tension: 0.3,
      },
    ],
  },
  options: {
    responsive: true,
    plugins: {
      title: { display: true, text: "全班均分趋势" },
      legend: { position: "bottom" },
    },
    scales: {
      y: { beginAtZero: false, min: 60, max: 100 },
    },
  },
});
```

## 柱状图代码模式

```javascript
new Chart(ctx, {
  type: "bar",
  data: {
    labels: ["数学", "语文", "英语", "物理", "化学"],
    datasets: [
      {
        label: "全班均分",
        data: [78, 82, 75, 70, 73],
        backgroundColor: COLORS.chart,
      },
    ],
  },
  options: {
    responsive: true,
    plugins: {
      title: { display: true, text: "各学科均分对比" },
    },
  },
});
```

## Dashboard 类框架

```javascript
class Dashboard {
  constructor(data) {
    this.rawData = data;
    this.filteredData = [...data];
    this.charts = {};
    this.init();
  }

  init() {
    this.populateFilters();
    this.renderKPIs();
    this.renderCharts();
    this.renderTable();
    document.getElementById("data-date").textContent =
      new Date().toLocaleDateString("zh-CN");
  }

  applyFilters() {
    const subject = document.getElementById("filter-subject")?.value;
    const grade = document.getElementById("filter-grade")?.value;

    this.filteredData = this.rawData.filter((row) => {
      if (subject && subject !== "all" && row.subject !== subject) return false;
      if (grade && grade !== "all" && row.grade !== grade) return false;
      return true;
    });

    this.renderKPIs();
    this.updateCharts();
    this.renderTable();
  }

  renderKPIs() {
    /* 更新 KPI 卡片 */
  }
  renderCharts() {
    /* 初始化图表 */
  }
  updateCharts() {
    /* 更新图表数据 */
  }
  renderTable() {
    /* 渲染数据表格 */
  }
  populateFilters() {
    /* 从数据中提取筛选选项 */
  }
}
```

## 响应式布局

```css
.dashboard-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}
.kpi-row {
  display: flex;
  gap: 16px;
  margin-bottom: 24px;
  flex-wrap: wrap;
}
.chart-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  margin-bottom: 24px;
}
.chart-card {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

@media (max-width: 768px) {
  .chart-row {
    grid-template-columns: 1fr;
  }
  .kpi-row {
    flex-direction: column;
  }
}

@media print {
  .filter-bar {
    display: none;
  }
  .dashboard-container {
    max-width: 100%;
    padding: 0;
  }
  .chart-card {
    break-inside: avoid;
    box-shadow: none;
    border: 1px solid #eee;
  }
}
```

## 数据表格

```html
<table class="data-table" id="detail-table">
  <thead>
    <tr>
      <th onclick="dashboard.sortTable(0)">姓名 ↕</th>
      <th onclick="dashboard.sortTable(1)">分数 ↕</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>
```

```css
.data-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}
.data-table th {
  background: #fafafa;
  padding: 12px;
  text-align: left;
  cursor: pointer;
}
.data-table td {
  padding: 10px 12px;
  border-bottom: 1px solid #f0f0f0;
}
.data-table tr:hover {
  background: #f5f5f5;
}
```
