# component-projects

👉 [English README](README.md) | 简体中文说明

一个包含有 `projects` 布局和 shortcode 的 Hugo 主题组件，用于显示 GitHub 固定卡片样式存储库。

![apple-devices-preview](https://github.com/hugo-fixit/component-projects/assets/33419593/3f75bd94-90df-4057-bee5-cbe2a61422f1)

## Demo

- <https://fixit.lruihao.cn/zh-cn/components/>
- <https://lruihao.cn/projects/>

## 要求

- [FixIt](https://github.com/hugo-fixit) v0.3.7 或更高版本

## 安装

将 [FixIt](https://github.com/hugo-fixit) 和此 git 存储库克隆到你的主题文件夹中，并将其添加为网站目录的子模块。

```bash
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
git submodule add https://github.com/hugo-fixit/component-projects.git themes/component-projects
```

接下来编辑项目的 `hugo.toml` 并将此主题组件添加到你的主题中：

```toml
theme = ["FixIt", "component-projects"]
```

最后，在 `layouts/partials/custom.html` 中的 `custom-head` 或 `custom-assets` 块内注入主题组件的样式：

```go-html-template
{{- define "custom-assets" -}}
  {{- partial "inject/component-projects.html" . -}}
{{- end -}}
```

要了解 Hugo 的主题组件以及如何使用它们，请查看 <https://gohugo.io/hugo-modules/theme-components/>。

## 配置

获取仓库信息依赖 GitHub 官方 API。在开始使用之前，建议在 GitHub 上生成个人访问令牌，以防止 GitHub API 使用限制。

1. 点击跳到 GitHub [生成 token](https://github.com/settings/tokens/new)，选择名为 `public_repo` 的范围以生成个人访问令牌。
2. 配置环境变量 `HUGO_PARAMS_GHTOKEN=your-person-access-token`，详细信息请参见 [os.Getenv | Hugo](https://gohugo.io/functions/os/getenv/#examples)

## 使用

### 布局

首先，创建 `projects.yml` 文件并编辑数据：

```bash
cp themes/component-projects/projects.yml.example data/projects.yml
```

> 如果你的网站是多语言的，你可以为英语创建一个 `projects.en.yml` 文件，为中文创建一个 `projects.zh-cn.yml` 文件。

接下来，使用 `projects` 布局创建一个新页面：

```bash
hugo new projects/_index.md
```

编辑新页面的标题和内容：

```yaml
---
title: My Projects
titleIcon: fa-solid fa-laptop-code
subtitle: <https://github.com/Lruihao>
sectionSlot: Some text to display in the section slot which is above the related articles list.
layout: projects
---

Some text to display at the start of the page.
```

### Shortcodes

在任何页面中使用 `gh-repo-card-container` 和 `gh-repo-card` 短代码来显示卡片式 GitHub 存储库：

```markdown
{{< gh-repo-card-container >}}
  {{< gh-repo-card repo="hugo-fixit/component-projects" >}}
  {{< gh-repo-card repo="Lruihao/hugo-blog" >}}
{{< /gh-repo-card-container >}}
```
