# 日常电影 · 个人摄影网站

用镜头定格日常的动容瞬间，以电影的方式记录一生。

网站基于 [Hugo](https://gohugo.io/) 和 [hugo-theme-gallery](https://github.com/nicokaiser/hugo-theme-gallery)（MIT 协议）搭建，由 GitHub Actions 自动发布到 GitHub Pages：

**https://cs-neil.github.io/Artist/**

## 目录结构

```
content/
├── _index.md            首页（标题、简介、封面 cover.jpg）
├── cover.jpg            首页封面，也用作社交分享预览图
├── chapters/            人生章节（多级相册：每年一个子相册）
│   ├── _index.md
│   ├── cover.jpg
│   └── 2026/
│       ├── index.md
│       └── 01-morning.jpg …
├── family/              家人 · 每年合影
├── empty-shots/         空镜（首页精选）
├── about.md             关于
├── artists.md           灵感 · 艺术家
└── cinematic.md         灵感 · 日常 × 电影感
hugo.toml                站点配置（标题、社交链接、图片质量等）
assets/css/custom.css    自定义样式
themes/gallery/          主题（git submodule，固定在 v4.9.3）
.github/workflows/       自动构建与发布
```

`content/` 里的照片目前都是**示例图片**，请换成你自己的照片。

## 添加照片

### 往已有相册里加照片

把 JPG 文件放进相册文件夹（比如 `content/empty-shots/`），推送后网站会自动更新。

- 照片默认按文件名排序，可以用 `01-`、`02-` 这样的前缀控制顺序。
- **不要使用 WebP 格式**：Hugo 缩放 WebP 图片时颜色会发灰，用 JPG 就好。
- 上传前建议把长边压到 2000–3000 像素，让仓库和网站保持轻量。

### 新建一个相册

新建一个文件夹，在里面放一个 `index.md` 和照片：

```
content/chapters/2027/
├── index.md
├── 01-xxx.jpg
└── 02-xxx.jpg
```

`index.md` 的写法：

```yaml
---
title: 2027 · 第二章
date: 2027-12-31
description: 这一章的简介，支持 Markdown。
resources:
  - src: 01-xxx.jpg
    title: 照片标题（在大图浏览时显示）
    params:
      cover: true      # 用这张作相册封面
---
```

常用选项：

| 选项 | 作用 |
|---|---|
| `weight` | 调整相册在列表里的顺序 |
| `params.featured: true` | 在首页顶部作为精选展示 |
| `params.private: true` | 不出现在相册列表里，只有知道链接的人能看 |
| `params.sort_by: Name` / `Date` | 照片按文件名或拍摄时间排序 |
| `params.sort_order: desc` | 倒序排列 |
| `params.theme: light` | 让这个相册用浅色背景 |
| `menus: main` | 把相册加到右上角菜单 |

更多用法见[主题文档](https://github.com/nicokaiser/hugo-theme-gallery#usage)。

### 隐私

- `hugo.toml` 里设置了不读取 GPS 坐标。
- `content/_index.md` 里设置了 `publishResources: false`：网站只发布压缩后的图片，不发布原图，也不带照片元数据（包括拍摄地点）。代价是访客无法下载原图；想开放下载，把它改成 `true`。

## 启用 GitHub Pages（只需设置一次）

1. 打开仓库的 **Settings → Pages**。
2. 把 **Source** 设为 **GitHub Actions**。
3. 到 **Actions** 页面重新运行 “Deploy site to GitHub Pages”，或者再推送一次。

之后每次推送到默认分支，网站都会自动重新构建和发布。推送到其他分支只会检查网站能否正常构建，不会发布。

## 本地预览

需要先安装 [Hugo Extended](https://gohugo.io/installation/) 0.123.0 或更新的版本（自动发布用的是 0.168.0）。

```sh
git clone --recursive https://github.com/CS-Neil/Artist.git
cd Artist
hugo server
```

然后在浏览器打开 http://localhost:1313/Artist/ 。

如果克隆时忘了加 `--recursive`，运行 `git submodule update --init` 把主题拉下来。

## 更新主题

```sh
cd themes/gallery
git fetch --tags
git checkout v4.x.x   # 换成新的版本号
cd ../..
git add themes/gallery
git commit -m "Update gallery theme"
```

更新后记得用 `hugo server` 本地预览一下，确认页面正常。
