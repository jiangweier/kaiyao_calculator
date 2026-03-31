# 项目记忆

## 项目概述

本项目为「开药计算器」。

## 已完成功能

- 开药计算器基础 HTML/JS 逻辑
- PWA 支持（可添加到手机桌面，离线可用）

## 基础设施

- 已配置 GitHub 远程仓库：https://github.com/jiangweier/kaiyao_calculator.git
- 已完成 Git 初始化及首次推送

## 技术细节

### 变量命名规范（JS）

- 驼峰命名：`visitDate`、`remainingPills`、`dailyDose`、`pillsPerBox`、`maxDays`
- `takeOnVisitDay`：复选框布尔值
- `plan`：开药计划数组，每项包含 `date`、`days`、`pills`、`boxes`、`endDate`
- 输入元素 ID 与变量名保持一致

### 核心公式逻辑

**按天数计算（calculateByDays）：**
- 需要药粒数 = `prescribeDays × dailyDose`
- 需要盒数 = `Math.ceil(pillsNeeded / pillsPerBox)`（向上取整）
- 实际开药粒数 = `boxesNeeded × pillsPerBox`
- 新药可吃天数 = `Math.floor(actualPills / dailyDose)`
- 勾选"问诊当天服药"时：总计可吃天数 `-1`

**按日期计算（buildPlan）：**
- 首次开药天数固定为最长开药天数（`maxDays`）
- 后续开药 = `min(剩余间隔天数 - 剩余药可覆盖天数, maxDays)`
- 首次覆盖天数 = `新开药可吃天数 + 剩余药可覆盖天数`（如有剩余）
- 勾选"问诊当天服药"：首次覆盖天数 `-1`

### 日期处理

- 使用自定义日期工具函数，**不依赖原生 `Date` 对象**（避免时区、夏令时问题）
- 日期格式：`{ year, month(0-based), day }`
- 工具函数：`createDate`、`addDays`、`daysBetween`、`dateLessThan`、`formatDate`
- 闰年判断：`(year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)`

### 两种计算模式

- `by-days`：输入开药天数，直接得出本次开药数量和建议下次开药日期
- `by-date`：输入下次问诊日期，自动拆解为多次开药计划并生成时间线

### 样式规范

- 单位：`px`（固定值，不使用 rem/vw/vh）
- 圆角：`8px`（小元素）、`12px`（卡片）、`16px`（容器）
- 主题色：`#4a90d9`（蓝色主色调）
- 成功色：`#27ae60`、警告色：`#fff3cd` / `#856404`
- 字体栈：`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`

### 已知兼容性处理

- 使用 `toISOString().split('T')[0]` 获取当天日期字符串，兼容 input[type="date"] 默认值
- checkbox 的 change 事件需同时监听 `input` 和 `change` 事件以确保所有浏览器触发
- 纯原生 HTML/CSS/JS，**无任何外部依赖**
