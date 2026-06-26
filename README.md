# ToolVerse — 部署与扩展指南

## 目录结构

```
toolverse/
├── index.html                  # 首页
├── tools/
│   └── random-picker/
│       └── index.html          # 自定义抽取器工具页
├── data/
│   └── tools.json              # 所有工具元数据（单一数据源）
├── locales/
│   ├── zh.json                 # 中文翻译
│   └── en.json                 # 英文翻译
├── public/
│   ├── robots.txt
│   └── sitemap.xml
└── nginx.conf                  # 宝塔 Nginx 配置参考
```

---

## 宝塔部署步骤

1. **上传文件**：将整个 `toolverse/` 上传至 `/www/wwwroot/newTrekking/`
2. **配置网站**：宝塔 → 网站 → 添加站点 → 域名填 `newTrekking.com`，根目录指向上述路径
3. **SSL 证书**：宝塔 → 网站 → 设置 → SSL → 申请 Let's Encrypt 免费证书
4. **Nginx 配置**：将 `nginx.conf` 内容替换到宝塔网站的「配置文件」中，保存并重载 Nginx
5. **提交收录**：
   - 登录 [Google Search Console](https://search.google.com/search-console) → 提交 `https://newTrekking.com/sitemap.xml`
   - 登录百度站长 → 同样提交 sitemap

---

## ✅ 添加新工具（两步完成）

### 第一步：在 `data/tools.json` 追加一条记录

```json
{
  "slug": "word-counter",
  "nameKey": "tool_word_counter_name",
  "descKey": "tool_word_counter_desc",
  "longDescKey": "tool_word_counter_longdesc",
  "category": "text",
  "categoryKey": "cat_text",
  "icon": "📝",
  "color": "blue",
  "keywords": ["字数统计", "word count"],
  "isNew": true,
  "isFeatured": false,
  "component": "WordCounter"
}
```

### 第二步：新建工具页面

复制 `tools/random-picker/index.html` 到 `tools/word-counter/index.html`，修改：
- `<title>` 和 meta description
- `<h1>` 工具名和描述
- JSON-LD 结构化数据
- 工具功能区 HTML/JS

### 第三步：更新多语言文件

在 `locales/zh.json` 和 `locales/en.json` 中追加新工具的翻译 key。

### 第四步：更新 sitemap.xml

在 `public/sitemap.xml` 中追加新工具的 `<url>` 块。

### 第五步：在首页添加工具卡片

在 `index.html` 的 `#tools-grid` 中复制并修改一个 `<article>` 卡片块。

---

## 多语言扩展

目前支持中文（zh）和英文（en）。添加新语言：

1. 在 `locales/` 下新建 `ja.json`（参照 `en.json` 结构翻译）
2. 在两个 HTML 文件的语言下拉菜单中取消注释对应语言选项

## 演示区
每个工具目录下新建 demo/ 文件夹，通过 manifest.json 控制：
tools/random-picker/
└── demo/
    ├── manifest.json   ← 配置文件
    ├── step1.png       ← 演示图片（按需放置）
    └── demo.mp4        ← 或视频
manifest.json 三种模式：
json{ "type": "images", "items": ["step1.png", "step2.png"] }  // 图片轮播
{ "type": "gif",    "items": ["demo.gif"] }                  // GIF 动图
{ "type": "video",  "items": ["demo.mp4"] }                  // 视频
items 为空数组时演示区完全不显示，不影响布局。
---

## SEO 维护清单

每次新增工具后：
- [ ] 工具页有独立 `<title>` 和 `<meta description>`（含目标关键词）
- [ ] 工具页有 JSON-LD 结构化数据
- [ ] sitemap.xml 已更新
- [ ] 在 Google Search Console 请求重新抓取
