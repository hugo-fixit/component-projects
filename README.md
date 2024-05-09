# component-projects

👉 English README | [简体中文说明](README.zh-cn.md)

A Hugo theme component with layout `projects` and shortcodes to display GitHub pinned card style repositories.

![apple-devices-preview](https://github.com/hugo-fixit/component-projects/assets/33419593/3f75bd94-90df-4057-bee5-cbe2a61422f1)

## Demo

- <https://fixit.lruihao.cn/components/>
- <https://lruihao.cn/projects/>

## Requirements

- [FixIt](https://github.com/hugo-fixit) v0.3.7 or higher

## Installation

Clone [FixIt](https://github.com/hugo-fixit) and this git repository into your theme folder and add it as submodules of your website directory.

```bash
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
git submodule add https://github.com/hugo-fixit/component-projects.git themes/component-projects
```

Next edit `hugo.toml` of your project and add this theme component to your themes:

```toml
theme = ["FixIt", "component-projects"]
```

Finally, inject the theme component's assets in `layouts/partials/custom.html` within the `custom-head` or `custom-assets` block:

```go-html-template
{{- define "custom-assets" -}}
  {{- partial "inject/component-projects.html" . -}}
{{- end -}}
```

To learn about theme components of hugo and how to use them, check out <https://gohugo.io/hugo-modules/theme-components/>.

## Configuration

Obtaining repositories information relies on GitHub official API. Before starting to use it, it is recommended to generate personal access token on GitHub to prevent GitHub API usage limit.

1. Go to your GitHub account settings, select the scope named `public_repo` to generate personal access token.
2. Configure with environment variable `HUGO_PARAMS_GHTOKEN=your-person-access-token`, see details in [os.Getenv | Hugo](https://gohugo.io/functions/os/getenv/#examples)

## Usage

### Layout

First, create the `projects.yml` file and edit your data:

```bash
cp themes/component-projects/projects.yml.example data/projects.yml
```

> If your site is multilingual, you can create a `projects.en.yml` file for English and `projects.zh-cn.yml` for Chinese.

Next, create a new page with the `projects` layout:

```bash
hugo new projects/_index.md
```

Edit the front matter and content of the new page:

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

Use the `gh-repo-card-container` and `gh-repo-card` shortcodes in any page to display a GitHub repository card:

```markdown
{{< gh-repo-card-container >}}
  {{< gh-repo-card repo="hugo-fixit/component-projects" >}}
  {{< gh-repo-card repo="Lruihao/hugo-blog" >}}
{{< /gh-repo-card-container >}}
```
