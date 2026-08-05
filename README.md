# KOKALabel报价系统

单文件 HTML 应用，将 Excel 纸张1号报价表导入网页端，支持多纸张报价计算、工艺/吊绳/邮费核算、客户等级管理及 Excel 导入导出。

## 线上地址

- **主域名**：https://tag-pricing-calculator.pages.dev
- **备用地址**：https://5c3a77e6.tag-pricing-calculator.pages.dev

## 当前版本

**v4.1.1**（2026-08-05）

## 功能概览

### 计算器页面
- 多纸张报价计算（纸张数量默认 2，可增减）
- 每张纸独立设置：纸张材质、尺寸(宽×长)、尺寸类型(单张/展开)、工艺(多选)
- 批量档位选择（下拉框 + 报价结果区药丸按钮快速切换）
- 吊绳选择、配送地区选择
- 报价结果明细：各纸张尺寸/面积/代码/单价表、纸张价合计(折扣前)、纸张折后价合计、工艺总费用、吊绳费用、邮费、成本合计
- 三档客户等级报价卡片（毛利系数）
- 小数位数自定义（0-4 位，默认 2 位）

### 1号报价表页面
- 纸张1号报价表查询（搜索、分页）
- 工艺价格表
- Excel 导入导出（纸张1号报价表、吊绳报价、邮费报价）

### 个人主页
- 公司信息设置（公司名、电话、默认档位、默认纸张、默认吊绳、小数位数）
- 客户等级管理（增删改、毛利系数）
- 数据管理（全量导出/导入、本地备份/恢复、快照创建/删除）
- 关于与版本信息

## 部署

- **入口**：`index.html`（与主文件 `tag-pricing-calculator-v4.0.html` 内容同步）
- **部署方式**：GitHub push → Cloudflare Pages 自动构建
- **构建配置**：无需 build，构建命令留空，输出目录为根目录

## 技术架构

- **单文件架构**：全部 HTML/CSS/JS 在一个 `.html` 文件中（约 5,700 行）
- **技术栈**：原生 HTML5 + CSS3（CSS 变量）+ 原生 JavaScript（ES6+）+ SheetJS（xlsx 0.20.1 CDN）
- **存储**：localStorage（客户等级、应用配置、快照、历史）

## 项目结构

```
.
├── index.html                          # 部署入口（主文件副本）
├── tag-pricing-calculator-v4.0.html    # 主程序（单文件应用）
├── robots.txt                          # 爬虫禁止（测试阶段）
├── CHANGELOG.md                        # 更新日志
├── README.md                           # 项目说明
└── docs/
    ├── AGENT-INSTRUCTIONS.md           # Agent 强制规则
    ├── HANDOFF-v4.1.1.md               # 项目交接文档
    └── tag-pricing-v4-analysis/        # 全流程分析报告
        └── tag-pricing-v4-analysis.html
```

## 核心原则

- 源 Excel 数据 100% 不修改
- 空值不补 0，统一用「无该批量定价」占位
- 不臆造数据，未匹配档位用 `null` 表示
- 所有 `innerHTML` 插入用户数据处使用 `escapeHtml()` 转义

## 相关文档

- [更新日志](CHANGELOG.md)
- [Agent 指令](docs/AGENT-INSTRUCTIONS.md)
- [项目交接文档](docs/HANDOFF-v4.1.1.md)
- [全流程分析报告](docs/tag-pricing-v4-analysis/tag-pricing-v4-analysis.html)
