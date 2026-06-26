# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目简述

纯前端微信公众号 Markdown 编辑器。用户在左侧写 Markdown，右侧实时预览，点击按钮复制富文本粘贴到公众号后台。无构建步骤、无后端，三个 JS 文件 + 一个 HTML 搞定。

## 启动与开发

```bash
# 启动本地服务（任选其一）
./start.sh
python3 -m http.server 8080
# 访问 http://localhost:8080
```

无 lint、无 test、无构建命令。改完刷新浏览器即可，注意浏览器缓存（脚本带 `?v=2` 版本号，必要时硬刷新或改版本号）。

## 三个核心文件

| 文件 | 角色 | 关键入口 |
|------|------|---------|
| `index.html` | UI + CSS + CDN 引入（Vue 3 / markdown-it / highlight.js / turndown / html2canvas） | 底部加载 `styles.js` 再加载 `app.js` |
| `styles.js` | 暴露全局 `STYLES` 对象，每个 key 是一种主题（当前 20 种，如 `wechat-default`），含 `name` + `styles`（每个 HTML 元素对应的内联 CSS 字符串） | 添加新样式：这里加配置 + `index.html` 的 `<select>` 加 `<option>` |
| `app.js` | Vue 应用 + 所有业务逻辑（~3500 行单文件） | 见下方架构说明 |

**注意**：`STYLES` 是 `styles.js` 定义的全局常量，`app.js` 的 Vue `data()` 里通过 `STYLES: STYLES` 暴露给模板。

## app.js 架构（单文件，按功能分段）

```
1-240     ImageStore 类：IndexedDB 封装（CRUD + 大小统计），DB=WechatEditorImages
245-340   ImageCompressor 类：Canvas API 压缩（max 1920px, quality 0.85）
345-620   ImageHostManager 类：已废弃的图床管理器，保留兼容
~625-770  Vue app setup: data()、mounted()（初始化 md/turndown/ImageStore）、computed、watch
770-1200  用户偏好、广告、文件上传、renderMarkdown()、scroll sync（编辑器↔预览行号联动）
1250-1700 渲染管线：processImageProtocol → applyInlineStyles → groupConsecutiveImages → convertGridToTable
1700-2300 copyToClipboard / copyToXArticles：复制到公众号 / 复制到 X Articles 长图
2300-2700 图片转 Base64、背景色提取、Turndown 初始化、智能粘贴（handleSmartPaste）
2770-2870 handleImageUpload、拖拽处理
2894-3340 小红书模式：html2canvas 分页截图导出
3340-3480 文章历史（localStorage）
```

## 三条关键数据流（改代码前先搞懂）

### 1. Markdown → 预览渲染
```
markdownInput → md.render() → processImageProtocol(img:// → blob:)
           → applyInlineStyles(根据 currentStyle 注入内联 CSS)
           → groupConsecutiveImages(连续图片组网格)
           → renderedContent
```

### 2. 图片粘贴/上传
```
File → ImageCompressor.compress (GIF/SVG 跳过) → 生成 img-{ts}-{rand} ID
    → ImageStore.saveImage 写入 IndexedDB
    → 编辑器插入 ![name](img://img-xxx)  ← 20 字符短链，避免大 base64 卡顿
    → 渲染时 processImageProtocol 从 IDB 读 Blob → Object URL 替换 src
    → img 标签带 data-image-id 供复制时使用
```

### 3. 复制到公众号（最关键也最易出 bug）
```
renderedContent HTML
  → convertGridToTable (CSS Grid → <table>，公众号不支持 Grid)
  → convertImageToBase64 (从 IDB 读 Blob → base64 data URL)
  → 包裹 section 容器（如有背景色）
  → 代码块简化 / 列表扁平化
  → Clipboard API 写入 text/html + text/plain
```
**核心约束**：公众号不支持 CSS Grid / Flexbox / CSS Variables / 类选择器 —— 所有样式必须 inline，布局必须 table。新增任何 CSS 特性前先想"公众号能不能识别"。

## 自定义协议 `img://`

编辑器里 Markdown 源文件中出现 `![...](img://img-xxx)`。这不是真 URL，而是 IndexedDB 主键。
- 预览期：`processImageProtocol()` 替换为 `blob:` Object URL（缓存在 `imageIdToObjectURL`）
- 复制期：`convertImageToBase64()` 通过 `data-image-id` 属性从 IDB 直接读原始 Blob 转 base64（不依赖网络 URL，避免 CORS）
- 后备路径：外链图片走 `fetch() → blob → base64`

改图片相关逻辑时，三个函数的语义边界（预览替换 / 复制转码 / IDB 读取）要分清。

## 样式系统约定

`styles.js` 里每个主题的 `styles` 对象必须包含以下 key（`applyInlineStyles()` 按这些 key 查找元素）：
`container, h1-h6, p, strong, em, a, ul, ol, li, blockquote, code, pre, hr, img, table, th, td, tr`

所有值都是**完整内联 CSS 字符串**，大量使用 `!important` 覆盖公众号后台自带样式。`img` 样式里 `max-height` 控制单张最大高度（默认 400-600px，网格内 360px）。

## 常见改动场景速查

- 加新样式主题 → `styles.js` 加条目 + `index.html` 的 `<select id="styleSelector">` 加 `<option>`
- 调图片最大高度 → 改 `styles.js` 各主题的 `img` 样式
- 改网格列数规则 → `app.js` 的 `groupConsecutiveImages()`
- 改 Grid→Table 转换 → `app.js` 的 `convertGridToTable()` / `convertToTable()`
- 调整压缩参数 → `app.js` mounted() 里 `new ImageCompressor({...})`
- 改复制后 HTML 结构 → `app.js` 的 `copyToClipboard()`

## 已知坑

- `app.js` 是 3500 行单文件，方法顺序即调用顺序，重排前要通读
- `ImageHostManager` 已废弃但保留在代码里，别被它误导
- `styles.js.backup` 和 `styles_temp.js` 是历史遗留，可忽略
- 修改后浏览器强缓存脚本，调试时硬刷新或改 HTML 里的 `?v=2` 版本号
- 公众号兼容性只能实机粘贴验证，预览看不出问题
