# 更新日志

所有 notable 变更均记录在此文件中。  
格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/)。

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
- HTML 添加更新日志弹窗（最近版本一览）
- 创建 `robots.txt` 禁止爬虫
- GitHub 仓库设为 Private

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
