# hugo-main

[tu-molo.github.io](https://tu-molo.github.io/) 的站点源码仓库。

线上站点由另一个仓库 [tu-MOLO/tu-MOLO.github.io](https://github.com/tu-MOLO/tu-MOLO.github.io) 拉取本仓库的 `main` 分支构建后发布，构建产物不提交到这里。

## 目录结构

| 路径 | 用途 |
|---|---|
| `content/post/` | 文章，每篇一个目录（Page Bundle） |
| `content/page/` | 固定页面：关于、归档、友链、搜索 |
| `content/categories/` | 分类页的封面与描述 |
| `hugo.yaml` | 站点配置：链接规则、菜单、侧边栏、公式开关等 |
| `themes/hugo-theme-stack/` | [Stack](https://github.com/CaiJimmy/hugo-theme-stack) 主题（随仓库一起版本化，非 submodule） |
| `layouts/`、`assets/` | 少量本地覆盖：自定义页脚、SCSS |
| `static/` | favicon、图标等直接复制到站点的文件 |
| `docs/` | 写作备忘，不参与构建 |

## 本地运行

需要 **Hugo 0.168.0 extended**（普通版无法编译 SCSS）。

```bash
hugo server -D
```

默认地址 <http://localhost:1313/>，`-D` 表示把草稿也渲染出来。

只生成静态文件：

```bash
hugo -D
```

输出到 `public/`，该目录已在 `.gitignore` 中。

## 新增文章

```bash
hugo new content/post/my-post/index.md
```

然后修改 front matter。几项必须注意：

- **`slug` 要手写英文短横线形式**。中文标题如果不写 `slug`，Hugo 会拿标题当路径，遇到 `/`、`++` 等字符会生成奇怪的 URL。
- `date` 使用 `YYYY-MM-DD`；`draft: true` 的文章在线上不会展示。
- `categories` 目前统一用 `项目`；`tags` 自由填写。
- 封面用 `image`，可以是图片文件（放在文章目录里）或外链。
- 写作细节和完整约定见 [docs/写作教程.md](docs/写作教程.md)。

## 发布

本仓库**没有**任何 workflow。发布链路固定在 [tu-MOLO/tu-MOLO.github.io](https://github.com/tu-MOLO/tu-MOLO.github.io)：

1. workflow 检出本仓库 `main`
2. 用 Hugo 0.168.0 extended 构建
3. 上传并部署到 GitHub Pages

本仓库推送后，下次定时构建（每天 02:00 北京时间）会带上新内容；想立刻发布，就去 io 仓库手动运行一次 `pages` 工作流。

## 技术栈

Hugo 0.168.0 extended + Stack 主题，KaTeX 公式，GitHub Pages 托管。

## 版权

文章内容除特别注明外均为本人原创，转载请署名并附原文链接。主题遵循其自身的开源协议。
