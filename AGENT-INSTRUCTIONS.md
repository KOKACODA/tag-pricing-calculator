# Agent 指令（必读）

> **任何 AI Agent 接手本项目时，必须先阅读此文件，并严格遵守以下规则。**

## 项目概述

- **项目名**：海洋吊牌报价系统
- **主文件**：`tag-pricing-calculator-v4.0.html`（单文件应用，含全部 HTML/CSS/JS）
- **部署入口**：`index.html`（与主文件保持同步）
- **部署平台**：Cloudflare Pages（通过 GitHub 自动构建）
- **GitHub 仓库**：`KOKACODA/tag-pricing-calculator`

## 强制规则

### 1. 每次修改必须更新 CHANGELOG.md

**无论修改大小，只要改动主文件代码，就必须在 `CHANGELOG.md` 顶部追加一条记录。**

格式：
```markdown
## [vX.Y] - YYYY-MM-DD

### 修复 / 新增 / 变更
- 具体描述改了什么，为什么改
```

版本号规则：
- 小修复 / Bug Fix：patch +1（如 v4.0 → v4.0.1）
- 新功能：minor +1（如 v4.0 → v4.1）
- 重大重构：major +1（如 v4.0 → v5.0）

### 2. index.html 必须与主文件同步

每次修改主文件后，执行：
```bash
cp tag-pricing-calculator-v4.0.html index.html
```

### 3. 提交规范

```bash
git add -A
git commit -m "vX.Y: 简述改动内容"
git push origin main
```

推送后 Cloudflare Pages 自动构建部署。

### 4. 不要在 HTML 中放更新日志

更新日志只放在 GitHub 的 `CHANGELOG.md` 中，HTML 内不内嵌更新日志。
HTML 中的 `noindex` / `nofollow` 元标签和 `robots.txt` 在测试阶段保持不变。

### 5. 测试阶段隐私策略

- HTML `<meta name="robots" content="noindex, nofollow, noarchive">` 保持不变
- `robots.txt` 的 `Disallow: /` 保持不变
- GitHub 仓库保持 Public（Cloudflare Pages 需要 GitHub 集成自动构建）
- 后续正式上线时，移除 noindex 标签和 robots.txt 限制即可

### 6. 代码质量要求

- 不要引入 TRAE IDE 的 CSS 变量（如 `--vscode-*`、`data-theme` 等）
- 所有 `innerHTML` 中插入用户数据时必须使用 `escapeHtml()` 转义
- `select` 元素只绑定 `change` 事件，不要同时绑定 `input`
- 及时删除无用函数和冗余代码
