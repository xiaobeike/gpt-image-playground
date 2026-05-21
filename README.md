# GPT Image Playground

在线地址：

- https://mofeite.qzz.io/
- https://xiaobeike.github.io/gpt-image-playground/

## 项目说明

这是一个纯前端静态网页（`index.html`），用于调用兼容 OpenAI 风格接口的图片生成与编辑能力。

当前线上正式发布走 `Cloudflare Pages`，仓库推送后自动更新，自定义域名为 `mofeite.qzz.io`。

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
- 发布方式：GitHub 仓库连接到 Cloudflare Pages
- 生产域名：`https://mofeite.qzz.io/`
- 备用地址：`https://xiaobeike.github.io/gpt-image-playground/`
- 修改后执行 `git push origin main`，Cloudflare Pages 会自动重新部署

## 已知问题与排查经验

### Edge 浏览器扩展可能导致 UI 文字刷新后失效

2026-05-21 遇到过一个已确认问题：

- 页面代码和 Cloudflare Pages 部署都正常
- DOM 里的文字实际存在
- 但用户自己的 Edge 浏览器里，部分按钮、标签、选项文字在刷新后会消失或变暗
- 点击语言切换后，文字会暂时恢复

根因不是站点部署失败，而是浏览器里的翻译类 / 阅读类扩展会在页面刷新后再次注入，二次改动 DOM 或样式，导致部分 UI 文本显示异常。

已观测到的扩展注入信号包括：

- `Immersive Translate`
- `doubao-ai-*`
- `ReaderArticleFinder / Readability`

### 下次再遇到同类问题，按这个顺序排查

1. 先确认线上是否已经是最新代码，不要一开始怀疑 Cloudflare 缓存。
2. 再确认 DOM 里文字是否还在。如果文字还在但视觉上不见了，问题多半是浏览器渲染层或扩展注入。
3. 如果现象是“刷新后坏、切换语言后好”，优先判断为扩展在 `load` 后二次篡改页面。
4. 优先做轻量修复：
   - 对关键 UI 文字保留 `notranslate` / `translate="no"`
   - 页面加载后多次重新执行 UI 文本恢复逻辑
5. 不要一开始就上伪元素复制文字这类重型 CSS 方案，容易引入叠字、错位和结构污染。

### 当前项目里针对这个问题的处理方式

- 关键 UI 文本统一放在 `.ui-label`
- 页面初始化、`load`、`pageshow`、页面重新可见时，会重复执行 UI 文本恢复
- 语言切换也会触发同一套恢复逻辑

### 经验结论

- “切换语言后恢复、刷新后再坏” 是浏览器扩展干扰的强信号
- 这类问题先看时序，再看 DOM，再看样式，不要先怀疑部署
- Cloudflare Pages 是否更新，应该用线上返回内容确认，不要凭感觉判断
