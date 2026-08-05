# 更新日志

所有 notable 变更均记录在此文件中。  
格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/)。

---

## [v4.3] - 2026-08-06

### 新增
- **三级数据架构明确**：在数据配置区新增架构层级注释和元信息常量
  - `GROUP_META`（小组：1号小组）→ `PRICE_LIST_META`（总报价表：1号报价表）→ Sheet 纸张报价表（PAPER_CONFIG）
  - 明确层级关系：小组 → 总报价表 → Sheet 纸张报价表（含规格 code × 档位价格）
  - 不改动任何现有数据和计算逻辑，仅增加架构概念定义
- **报价表查询页纸张选择器**：新增下拉选择器，可直接选择任意 Sheet 纸张查看报价表
  - Sheet 数量 > 10 时自动显示编号前缀（如"1. 350铜版纸"），方便快速定位
  - 保留原有上一张/下一张按钮，两种导航方式并存
- **计算器纸张下拉编号**：计算器页面的纸张材质下拉在 Sheet > 10 张时也显示编号前缀

### 变更
- **Excel 导出/模板新增架构层级信息**：每个 Sheet 的元数据区新增"所属小组"和"总报价表"两行
  - `paperToSheetRows()` 输出新增 2 行架构元信息
  - `downloadPaperTemplate()` 9 张模板表均新增架构元信息
  - `parsePaperExcel()` 改为按标签匹配读取（不再依赖固定行号），兼容新旧格式
  - 模板 Sheet 命名改为从 meta 中提取"简称"，不再依赖固定索引
- **报价表查询页分页信息优化**：`paperPageInfo` 显示格式从"1 / 9"改为"第 1 / 9 张"

---

## [v4.2.1] - 2026-08-05

### 变更
- **报价表重命名**：当前报价表数据为单一小组数据，全站"报价表"更名为"1号报价表"（导航栏、卡片标题、纸张名称、Excel 导入导出文件名、代码注释等共 77 处）
- **公司名替换**：全站"海洋吊牌厂"/"海洋吊牌"替换为"KOKALabel"（系统标题、个人主页、公司名默认值、导出文件名、交接文档、分析报告等共 23 处）

---

## [v4.2] - 2026-08-05

### 新增
- **全端响应式 UI 适配**：实现 Web 桌面、iOS 移动端、安卓移动端全平台响应式布局
  - 根字体 `clamp(13px, calc(13px + 0.4vw), 16px)` 响应式缩放，所有字体改用 rem/vw 相对单位，禁止硬编码 px
  - 所有容器、卡片、模块使用 `clamp()` 相对单位自适应，禁止固定宽高
  - 新增 3 个响应式断点：平板(≤768px)、手机(≤480px)、超小屏(≤360px)
  - iOS 安全区域适配（`env(safe-area-inset-*)`），支持刘海屏/底部安全区
- **下拉弹窗视口边界检测**：纸张下拉弹窗打开时自动检测视口边界
  - 下方空间不足时向上翻转（flip-up）
  - 右侧溢出时左移（shift-left）
  - 超小屏弹窗超出容器时切换全屏固定定位（full-width）
  - 滚动/旋转屏幕时自动关闭下拉，避免定位错乱
- **盒子溢出防护**：全局 `overflow-x: hidden`、`overflow-wrap: break-word`，卡片容器 `overflow: hidden`，文本节点 `text-overflow: ellipsis`，防止内容溢出
- **图形组件防溢出**：`img/video/canvas/svg { max-width: 100%; height: auto }` 通用规则

---

## [v4.1.2] - 2026-08-05

### 变更
- **仓库文件整理**：将 `AGENT-INSTRUCTIONS.md`、`HANDOFF-v4.1.1.md`、`tag-pricing-v4-analysis/` 归档至 `docs/` 文件夹，根目录仅保留部署必需文件和 `README.md`、`CHANGELOG.md`
- **重写 README.md**：更新版本号至 v4.1.1，补充 v4.0+ 全部功能描述、项目结构树、技术架构说明、相关文档链接，修正过时的文件引用
- **更新文档路径引用**：`HANDOFF-v4.1.1.md` 和 `AGENT-INSTRUCTIONS.md` 内的文件路径同步更新为 `docs/` 下的新路径

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
