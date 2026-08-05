# 更新日志

所有 notable 变更均记录在此文件中。  
格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/)。

---

## [v4.1.1] - 2026-08-05

### 变更
- **纸张价合计显示优化**：多纸张时改为"¥纸张1价格 + ¥纸张2价格 = ¥合计"格式，合计用蓝色加粗字体；单纸张时直接显示价格，不加等式

---

## [v4.1] - 2026-08-05

### 新增
- **报价结果增加纸张价合计（折扣前）**：在"纸张折后价合计"上方新增一行，显示未打折的纸张原价合计，方便对比折扣幅度
- **批量档位快速切换**：报价结果区域新增药丸式按钮组，点击即可快速切换不同批量档位重新计算，当前档位高亮显示，无需回到顶部操作下拉框

### 变更
- **纸张数量默认值从 1 改为 2**：页面加载时默认显示 2 张纸的设置卡片

---

## [v4.0.1] - 2026-08-05

### 变更
- **移除 HTML 内更新日志弹窗**：取消 HTML 中的 CHANGELOG 数组、showChangelog/hideChangelog 函数、弹窗 CSS 和 UI 按钮，更新日志改为仅由 GitHub `CHANGELOG.md` 维护
- **GitHub 仓库改回 Public**：Private 仓库会导致 Cloudflare Pages 的 GitHub 集成断开，改回 Public 并依赖 `noindex` + `robots.txt` 防搜索引擎收录
- **新增个人主页测试阶段提示**：关于与版本卡片中增加"测试阶段提示"行，标注当前已屏蔽搜索引擎收录
- **新增 AGENT-INSTRUCTIONS.md**：项目根目录新增 Agent 指令文件，确保任何 AI Agent 接手时自动更新 CHANGELOG.md、同步 index.html、遵守代码质量规则

---

## [v4.0] - 2026-08-05

### 修复
- **删除 TRAE IDE CSS 污染**：清除 1,213 行无关 CSS（VSCode 主题变量、data-theme 属性等），文件体积从 289KB 降至 196KB（减少 32%）
- **修复报价表小数位硬编码**：`renderPriceTable()` 中两处 `toFixed(2)` 替换为 `formatMoney()`，统一走 `parseDecimalPlaces` 逻辑
- **修复快照数据不完整**：`createSnapshot()` 补充 `ropeConfig`、`craftConfig`、`shippingConfig`，同时修复 `exportFullData()` 和导入函数同步支持
- **修复 XSS 风险**：在所有 `innerHTML` 动态插入用户数据处加 `escapeHtml()` 转义，覆盖 6 个渲染函数共 12 处
- **修复事件重复绑定**：`select` 元素（档位、地区、尺寸类型）从 `input`+`change` 双绑定改为仅 `change`，避免重复计算
- **删除死函数**：移除从未调用的 `matchTier()` 和 `getPriceByTier()`

### 新增
- HTML 添加 `noindex`/`nofollow` 元标签，防止搜索引擎收录
- 创建 `robots.txt` 禁止爬虫

---

## [v3.9] - 2026-08-04

### 修复
- **修复小数位数 0 值 NaN 崩溃**：`parseInt("")` 返回 NaN 导致 `toFixed(NaN)` 报错
- 引入 `parseDecimalPlaces()` 安全解析函数：0 有效，NaN/空值回退 2，范围 0-4

---

## [v3.8] - 2026-08-04

### 新增
- 禁用数字输入框鼠标滚轮调节（`onwheel="return false"`）
- 邮费 Excel 导入导出功能
- 邮费管理 UI 面板

### 修复
- 修复 `APP_PROFILE` 持久化失败问题
- 修复小数位 0 值失效

---

## [v3.4] - 2026-08-03

### 初始版本
- 基础报价计算引擎（纸张 + 工艺 + 吊绳 + 邮费 × 毛利系数）
- 9 张纸张报价表、工艺配置、吊绳配置、邮费配置
- 三页面架构（计算器 / 报价表 / 个人主页）
- Excel 报价表导入导出（SheetJS）
- localStorage 持久化（客户等级、应用配置、快照、历史）
- Cloudflare Pages 部署
