# 公众号 Markdown 编辑器

<div align="center">
  <img src="logo.svg" width="120" height="120" alt="公众号 Markdown 编辑器">

  <h3>一个专为微信公众号设计的 Markdown 编辑器</h3>
  <p><strong>二次开发版本</strong> - 基于 <a href="https://github.com/alchaincyf/huasheng_editor">huasheng_editor</a> 重构</p>

  [![在线体验](https://img.shields.io/badge/在线体验-markdown.wc.cd-0066FF?style=for-the-badge)](https://markdown.wc.cd/)
  [![样式数量](https://img.shields.io/badge/样式-31_种-ff6b6b?style=for-the-badge)](#-样式主题)
  [![许可证](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)](LICENSE)
</div>

## 🌟 在线体验

👉 **[https://markdown.wc.cd/](https://markdown.wc.cd/)**

## 📋 项目说明

本项目是 [huasheng_editor](https://github.com/alchaincyf/huasheng_editor) 的二次开发版本，在原有功能基础上进行了重大重构和功能扩展：

### 🔄 主要改进

- **UI 重构**：Dell 1996 复古设计系统，三栏布局
- **样式扩展**：从 13 种增加到 31 种精心设计的样式主题
- **长文优化**：所有样式针对长文阅读进行优化
- **布局改进**：侧栏样式选择器，顶部对齐的三列布局

## ✨ 功能特点

### 🎨 31 种精美样式

**经典媒体系列**：
- 金融时报、纽约时报、Le Monde 世界报、Nikkei 日経、Guardian 卫报

**科技品牌系列**：
- Anthropic Claude、Cursor、Expo、Figma、Framer、HashiCorp、IBM、Intercom、ElevenLabs、Cohere

**设计工具系列**：
- Airbnb、Airtable、Clay、Discord、Cal.com

**汽车品牌系列**：
- BMW、BMW M、Bugatti、Ferrari

**复古/编辑系列**：
- Dell 1996、原研哉·空、Hische·编辑部、安藤·清水、高迪·有机

**其他精选**：
- Binance、ClickHouse、Jony Ive、Apple 极简、Medium 长文、深度阅读、优雅简约、技术风格、默认公众号风格、档案馆、焦橙文档

### 🖥️ Dell 1996 设计系统

采用 Dell 1996 复古设计语言：
- **黑色页框**：8px 黑色边框围绕整个视口
- **色块系统**：flat color blocks，无渐变无阴影
- **字体栈**：Arial Black / Helvetica / Times New Roman
- **0px 圆角**：所有元素直角设计
- **硬边投影**：4px 4px 0 黑色硬阴影

### 📐 三栏布局

- **左侧边栏**：样式选择器（垂直滚动）+ 上传按钮
- **中间编辑器**：Markdown 编辑区 + 字符统计
- **右侧预览**：实时预览 + 操作按钮
- **顶部对齐**：三列顶部高度一致（56px）

### 📸 智能图片处理

- **智能粘贴**：支持从任何地方粘贴图片（截图、浏览器、文件管理器）
- **自动压缩**：图片自动压缩到合理大小（最高压缩 80%+）
- **本地存储**：使用 IndexedDB 持久化存储，刷新不丢失
- **编辑友好**：编辑器中使用短链接（`img://img-xxx`），不会卡顿
- **多图网格**：2-3 列自动排版，类似朋友圈
- **完美兼容**：复制到公众号时自动转 Base64

### 📝 长文优化

所有样式针对长文阅读优化：
- **Georgia 衬线字体**：经典编辑感
- **暖奶油色背景**：#faf8f3，减少视觉疲劳
- **行高 1.7**：适合长时间阅读
- **结构装饰**：边框替代渐变/阴影
- **克制色彩**：5-7 种颜色，单一强调色

### 🚀 强大功能

- **实时预览**：左侧编辑，右侧即时查看效果
- **一键复制**：直接粘贴到公众号编辑器，格式完美保留
- **智能粘贴**：支持从飞书、Notion、Word 等富文本应用直接粘贴
- **图片拖拽**：支持拖拽图片文件到编辑器
- **样式收藏**：收藏常用样式，快速切换
- **文件上传**：支持 .md / .markdown 文件
- **代码高亮**：优雅的代码块展示，支持多种语言
- **滚动同步**：编辑器和预览区滚动位置同步
- **响应式设计**：完美适配桌面、平板、手机

## 📖 使用指南

### 快速开始

1. 访问 [在线编辑器](https://markdown.wc.cd/)
2. 在中间输入或粘贴 Markdown 内容
3. 在左侧选择喜欢的样式主题
4. 点击右侧「复制到公众号」
5. 粘贴到微信公众号编辑器

### 本地运行

```bash
# 克隆仓库
git clone https://github.com/seventhocean/winter-mdwechat.git

# 进入目录
cd winter-mdwechat

# 启动本地服务器（Python）
python3 -m http.server 8080

# 或使用提供的脚本
./start.sh

# 访问 http://localhost:8080
```

## 🛠️ 技术栈

- **Vue 3** - 渐进式前端框架
- **Markdown-it** - 强大的 Markdown 解析器
- **Highlight.js** - 代码语法高亮
- **IndexedDB** - 本地图片持久化存储
- **Canvas API** - 客户端图片压缩
- **Turndown** - HTML 转 Markdown（智能粘贴）
- **html2canvas** - 小红书图片导出
- **纯 CSS** - 无需构建工具，开箱即用

## 📂 项目结构

```
winter-mdwechat/
├── index.html        # 主页面（Dell 1996 设计系统）
├── app.js           # Vue 应用逻辑
├── styles.js        # 31 种样式主题配置
├── DESIGN.md        # Dell 1996 设计规范
├── *.md             # 42 个品牌设计文档
├── icon.svg         # 项目图标
├── favicon.svg      # 网站图标
├── logo.svg         # Logo 图标
├── start.sh         # 启动脚本
├── README.md        # 项目说明
├── CLAUDE.md        # 技术文档
└── LICENSE          # 开源许可证
```

## 💡 核心特性

### ⭐ 图片处理系统

**技术架构**：
```
用户粘贴图片
    ↓
Canvas API 压缩（最大 1920px，质量 85%）
    ↓
IndexedDB 持久化存储
    ↓
编辑器显示短链接（img://img-xxx）
    ↓
预览区从 IndexedDB 加载显示
    ↓
复制时自动转 Base64
```

**核心优势**：
- ✅ **100% 成功率**：不依赖外部图床，完全本地化
- ✅ **编辑器流畅**：短链接不会造成卡顿
- ✅ **刷新不丢失**：IndexedDB 持久化存储
- ✅ **智能压缩**：平均压缩 50%-80%
- ✅ **跨平台支持**：支持截图、浏览器、文件管理器等所有粘贴来源

**多图网格布局**：
- 连续 2 张图片：并排两列展示
- 连续 3 张图片：一行三列展示
- 连续 4 张图片：2×2 网格
- 5 张及以上：3 列网格布局

### 公众号完美兼容

- ✅ 自动将 CSS Grid 转换为 Table 布局
- ✅ 所有样式转为内联样式
- ✅ 图片自动转 Base64
- ✅ 强制样式优先级（!important）

### 推荐样式

**编辑风格**（适合长文）：
- **Anthropic Claude** - 温暖人文的 AI 风格
- **金融时报** - 专业的财经风格
- **纽约时报** - 经典的新闻风格
- **Dell 1996** - 复古企业风格

**品牌风格**：
- **Figma** - 设计工具的 playful 感
- **Ferrari** - 奢华赛车红
- **IBM** - 企业级权威感
- **原研哉·空** - 日式极简美学

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

### 如何贡献

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交你的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

### 添加新样式

1. 在 `styles.js` 中添加新的样式配置
2. 确保包含所有必需的元素样式（23 个 key）
3. 测试各种 Markdown 元素的渲染效果
4. 提交 PR 并附上效果截图

## 🙏 致谢

- **原作者**：[花生](https://github.com/alchaincyf) - [huasheng_editor](https://github.com/alchaincyf/huasheng_editor)
- **设计文档**：使用 [getdesign](https://www.npmjs.com/package/getdesign) 工具收集品牌设计规范
- **二次开发**：[seventhocean](https://github.com/seventhocean)
- 感谢所有贡献者和使用者

## 📄 开源协议

本项目基于 [MIT License](LICENSE) 开源。

你可以自由地：
- ✅ 商业使用
- ✅ 修改
- ✅ 分发
- ✅ 私有使用

## 📊 更新日志

### v2.0 (2026-03-25)
- 🎨 UI 重构为 Dell 1996 设计系统
- 📐 三栏布局重构
- 🎯 新增 20 个品牌样式
- ✨ 优化 16 个样式为长文编辑风格
- 📝 总计 31 个样式

### v1.0 (原始版本)
- 基础 Markdown 编辑功能
- 13 种预设样式
- 图片处理和压缩
- 智能粘贴功能

---

<div align="center">
  <p><strong>二次开发版本</strong> - 基于 <a href="https://github.com/alchaincyf/huasheng_editor">huasheng_editor</a></p>
  <p>Made with ❤️ by <a href="https://github.com/seventhocean">seventhocean</a></p>
  <p>如果觉得有用，请给个 ⭐️ Star 支持一下！</p>
</div>
