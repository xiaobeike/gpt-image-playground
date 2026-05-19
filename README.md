# GPT Image Playground

在线地址（GitHub Pages）：

- https://xiaobeike.github.io/gpt-image-playground/

## 项目说明

这是一个纯前端静态网页（`index.html`），用于调用兼容 OpenAI 风格接口的图片生成与编辑能力。

支持：

- 文生图（`/images/generations`）
- 图片编辑（`/images/edits`）
- 中文提示词自动翻译（`/chat/completions`）
- 本地历史记录、提示词历史、主题切换

## 使用方式

1. 打开在线页面
2. 点击“配置接口”
3. 填写：
   - `Base URL`（如 `https://api.openai.com/v1`）
   - `API Key`
4. 保存后即可生成或编辑图片

## 安全说明

- API Key 保存在**当前用户自己的浏览器本地存储（localStorage）**中。
- 仓库源码未内置你的真实 Key。
- 公开页面场景下，请不要把你个人长期 Key 提供给他人共用。

## 本地开发与发布

- 主文件：`index.html`
- 发布方式：GitHub Pages（`main` 分支，`/` 根目录）
- 修改后执行 `git push`，页面会自动更新
