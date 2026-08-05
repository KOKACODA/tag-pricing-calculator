# 项目转接文档 — 海洋吊牌报价系统 v4.1.1

> **此文件用于新对话窗口接手，请先完整阅读此文档，再阅读 `docs/AGENT-INSTRUCTIONS.md`。**

---

## 一、项目基本信息

| 项目 | 值 |
|------|-----|
| 项目名 | 海洋吊牌报价系统 |
| 当前版本 | v4.1.1 |
| 主文件 | `tag-pricing-calculator-v4.0.html`（5,693 行，198KB） |
| 部署入口 | `index.html`（与主文件同步） |
| GitHub 仓库 | `KOKACODA/tag-pricing-calculator`（Public） |
| 线上地址 | https://tag-pricing-calculator.pages.dev |
| 备用地址 | https://5c3a77e6.tag-pricing-calculator.pages.dev |
| 部署方式 | GitHub push → Cloudflare Pages 自动构建 |
| GitHub Token | （见 git remote 配置，不在此明文记录） |
| 用户称呼 | Master |

---

## 二、工作区文件清单

| 文件 | 用途 |
|------|------|
| `tag-pricing-calculator-v4.0.html` | 主程序（单文件应用，含全部 HTML/CSS/JS） |
| `index.html` | 部署入口（主文件副本，Cloudflare Pages 索引） |
| `CHANGELOG.md` | 完整更新日志（v3.4 → v4.1.1） |
| `robots.txt` | 禁止爬虫（测试阶段） |
| `README.md` | 项目说明 |
| `docs/AGENT-INSTRUCTIONS.md` | Agent 强制规则（必读） |
| `docs/HANDOFF-v4.1.1.md` | 项目交接文档（本文件） |
| `docs/tag-pricing-v4-analysis/tag-pricing-v4-analysis.html` | 全流程分析报告（架构/机制/修复/优化方向） |

---

## 三、版本历史摘要

| 版本 | 日期 | 关键改动 |
|------|------|----------|
| v3.4 | 2026-08-03 | 初始版本，基础报价计算 + Excel 导入导出 |
| v3.8 | 2026-08-04 | 禁用滚轮调数、邮费 Excel 导入导出 |
| v3.9 | 2026-08-04 | 修复小数位 0 值 NaN 崩溃 |
| v4.0 | 2026-08-05 | 删除 TRAE CSS 污染(1213行)、修复 XSS/快照/小数位/事件绑定/死函数 |
| v4.0.1 | 2026-08-05 | 移除 HTML 更新日志弹窗、仓库改回 Public、新增 AGENT-INSTRUCTIONS.md |
| **v4.1** | 2026-08-05 | **新增纸张价合计(折扣前)、批量档位快速切换、纸张数量默认 2** |
| **v4.1.1** | 2026-08-05 | **纸张价合计显示优化：多纸张分张价格+合计(蓝字)** |

---

## 四、当前功能清单

### 计算器页面
- 多纸张报价计算（纸张数量默认 2，可增减）
- 每张纸独立设置：纸张材质、尺寸(宽×长)、尺寸类型(单张/展开)、工艺(多选)
- 批量档位选择（下拉框 + 报价结果区药丸按钮快速切换）
- 吊绳选择、配送地区选择
- 报价结果明细：各纸张尺寸/面积/代码/单价表、纸张价合计(折扣前)、纸张折后价合计、工艺总费用、吊绳费用、邮费、成本合计
- 多纸张时纸张价显示格式：`¥价格1 + ¥价格2 = ¥合计`（合计蓝色加粗）
- 三档客户等级报价卡片（毛利系数）
- 小数位数自定义（0-4 位，默认 2 位）

### 报价表页面
- 纸张报价表查询（搜索、分页）
- 工艺价格表
- Excel 导入导出（纸张报价表、吊绳报价、邮费报价）

### 个人主页
- 公司信息设置（公司名、电话、默认档位、默认纸张、默认吊绳、小数位数）
- 客户等级管理（增删改、毛利系数）
- 数据管理（全量导出/导入、本地备份/恢复、快照创建/删除）
- 关于与版本（当前版本、数据存储方式、测试阶段提示）

---

## 五、技术架构

- **单文件架构**：全部 HTML/CSS/JS 在一个 `.html` 文件中
- **技术栈**：原生 HTML5 + CSS3（CSS 变量） + 原生 JavaScript（ES6+） + SheetJS（xlsx 0.20.1 CDN）
- **存储**：localStorage（客户等级、应用配置、快照、历史）；报价配置测试阶段不持久化（EPHEMERAL_KEYS 机制）
- **部署**：GitHub → Cloudflare Pages 自动构建

### 代码分区（行号范围）
| 区域 | 行范围 | 内容 |
|------|--------|------|
| CSS 样式 | 1 - 1035 | 全局变量、布局、组件、响应式 |
| HTML 骨架 | 1036 - 1830 | 导航栏、3 个页面、设置面板 |
| 数据配置层 | 1831 - 3350 | 9 张纸张报价表、工艺、吊绳、邮费 DEFAULT 常量 |
| 计算逻辑层 | 3351 - 3500 | 面积计算、规格匹配、档位匹配、成本核算 |
| 交互渲染层 | 3501 - 5693 | DOM 渲染、事件绑定、Excel 导入导出、初始化 |

---

## 六、测试阶段隐私策略

- HTML `<meta name="robots" content="noindex, nofollow, noarchive">` — 搜索引擎不收录
- `robots.txt` → `Disallow: /` — 爬虫禁止访问
- GitHub 仓库保持 **Public**（Private 会导致 Cloudflare Pages GitHub 集成断开）
- 后续正式上线时：移除 noindex 标签 + 删除 robots.txt 限制即可

---

## 七、已知技术债 / 待优化项

| 项目 | 优先级 | 说明 |
|------|--------|------|
| 快照恢复功能 | 中 | 有创建/删除快照，缺"恢复到指定快照"按钮 |
| 历史记录假数据 | 中 | `addHistoryPlaceholder()` 只添加假数据，需对接真实计算结果 |
| 邮费多档位逻辑 | 中 | 邮费以第一张纸面积判断小面积折扣，多纸张应改总面积 |
| 云同步占位 | 低 | 4 个功能卡片均为 `showToast('占位')` |
| EPHEMERAL_KEYS 机制 | 低 | 测试阶段临时方案，正式版应移除 |
| const SHIPPING_CONFIG | 低 | 用 `length=0; push()` 替换内容，建议改 `let` 直接赋值 |
| 文件拆分 | 低 | 5,693 行单文件，建议未来拆分为多文件 + 构建工具 |

---

## 八、新对话接手步骤

1. **读取 `docs/AGENT-INSTRUCTIONS.md`** — 了解强制规则
2. **读取 `CHANGELOG.md`** — 了解完整版本历史
3. **读取主文件 `tag-pricing-calculator-v4.0.html`** — 用 Grep/Read 查看具体代码段
4. **修改代码后必须执行**：
   ```bash
   cp tag-pricing-calculator-v4.0.html index.html
   # 更新 CHANGELOG.md 顶部追加新版本记录
   git add -A
   git commit -m "vX.Y: 简述改动"
   git push origin main
   ```
5. **等待 60-90 秒后验证部署**：
   ```bash
   curl -s "https://tag-pricing-calculator.pages.dev?cb=$(date +%s)" | grep '关键词'
   ```

---

## 九、关键函数索引

| 函数名 | 大致行号 | 用途 |
|--------|----------|------|
| `calculate()` | 3361 | 核心计算引擎 |
| `onCalculate()` | 3870 | 收集输入并调用 calculate + 渲染结果 |
| `renderPaperTotal()` | 3927 | 纸张价合计渲染（多纸张分张+合计） |
| `renderSheets()` | 3704 | 纸张设置卡片渲染 |
| `renderPriceTable()` | 4010 | 报价表页面渲染 |
| `updateTierOptions()` | 3668 | 更新批量档位下拉选项 |
| `formatMoney()` | 3360 | 格式化金额（尊重小数位数） |
| `parseDecimalPlaces()` | 3350 | 安全解析小数位数 |
| `escapeHtml()` | 3758 | XSS 转义 |
| `bindEvents()` | 5514 | 全局事件绑定 |
| `safeInit()` | 5560 | 启动初始化 |

---

*文档生成时间：2026-08-05 | 版本：v4.1.1 | 生成者：TRAE Agent*
