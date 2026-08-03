# 海洋吊牌报价系统 v3.3 变更说明

> 日期：2026-08-03
> 文件：`tag-pricing-calculator-v3.3.html` / `index.html`

## 一、本次变更

### 1.1 吊绳默认改为「普通吊绳」

- 打开计算器时，吊绳单选默认勾选 `rope1`（普通吊绳）。
- 之前默认是 ROPE_CONFIG 第 0 项（"不加吊绳"），现在改为「普通吊绳」。

### 1.2 个人主页「公司信息与报价单设置」新增两项

新增两个 setting-row：

- **默认吊绳类型**（下拉）
  - 选项动态生成自 ROPE_CONFIG。
  - 选中后保存到 `APP_PROFILE.defaultRope`。
  - 默认空选项显示「跟随默认（普通吊绳）」，表示按「普通吊绳」兜底。
- **默认纸张材质**（下拉）
  - 选项动态生成自 PAPER_CONFIG，显示各纸张的简称（`shortName`）。
  - 选中后保存到 `APP_PROFILE.defaultPaperId`。
  - 默认空选项显示「跟随默认（报价表 3）」，即沿用之前 `currentPaperIndex = 2` 的行为。

## 二、行为说明

- 页面首次加载（`bindEvents()` 之后）：
  - 若 `APP_PROFILE.defaultRope` 在 ROPE_CONFIG 中存在，吊绳默认勾选该项；否则回退到「普通吊绳」，再否则回退到第一项。
  - 若 `APP_PROFILE.defaultPaperId` 在 PAPER_CONFIG 中存在，定位 `currentPaperIndex` 并触发对应 UI 更新（默认调用 `updatePaperUI` / `renderPaper`）。
- 保存设置：`保存设置` 按钮会把新值写入 `localStorage["appProfile"]`，并立即重算报价。
- 旧版 `appProfile` 配置自动兼容：
  - 缺失 `defaultRope` 时自动填充为 `rope1`（普通吊绳）。
  - 缺失 `defaultPaperId` 时默认为空，沿用「报价表 3」默认行为。
- 吊绳 / 纸张配置通过 Excel 重新导入后，调用的 `rebuildRopeUI` / 重建流程仍按个人主页设置决定默认勾选项。

## 三、兼容性

- 旧版（v3.1 / v3.2）导出的本地备份文件可被 v3.3 正常识别。
- 旧版（v3.1 / v3.2）导出的完整配置 JSON 文件可被 v3.3 正常导入。
- `localStorage` 中旧版 `appProfile` 字段无需迁移，运行时自动补齐。

## 四、文件清单

- 新增：`tag-pricing-calculator-v3.3.html`（本次主版本）
- 同步：`index.html`（部署入口，与 v3.3 内容一致）
- 变更说明：`CHANGELOG-v3.3.md`（本文档）
- 旧版保留：`tag-pricing-calculator-v3.1.html`（v3.2 中间版本按 Master 规则清理）
