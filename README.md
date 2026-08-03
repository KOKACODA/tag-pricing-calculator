# 海洋吊牌报价系统

单文件 HTML 应用，把 Excel 报价表（9 张纸张）导入到网页端，可查询、可计算、可导出报价单。

## 部署

- 入口：`index.html`（与 `tag-pricing-calculator-v3.4.html` 内容一致）
- Cloudflare Pages 部署：直接发布根目录，**无需 build**，构建命令留空
- 详细变更：`CHANGELOG-v3.4.md` / 项目状态：`PROJECT-STATUS.md` / 交接说明：`HANDOFF.md`

## 铁律

- 源 Excel 100% 不修改
- 空值不补 0，统一用「无该批量定价」占位
- 不臆造数据，未匹配档位用 `null` 表示

## 当前版本

v3.4.2
