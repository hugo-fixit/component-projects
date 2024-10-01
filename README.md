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

The installation method is the same as [installing a theme](https://fixit.lruihao.cn/documentation/installation/). There are several ways to install, choose one.

### Install as Hugo Module

First make sure that your project itself is a [Hugo module](https://gohugo.io/hugo-modules/use-modules/#initialize-a-new-module).

Then add this theme component to your `hugo.toml` configuration file:

```toml
[module]
  [[module.imports]]
    path = "github.com/hugo-fixit/FixIt"
  [[module.imports]]
    path = "github.com/hugo-fixit/component-projects"
```

On the first start of Hugo it will download the required files.

To update to the latest version of the module run:

```bash
hugo mod get -u
hugo mod tidy
```

### Install as Git Submodule

Clone [FixIt](https://github.com/hugo-fixit) and this git repository into your theme folder and add it as submodules of your website directory.

```bash
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
git submodule add https://github.com/hugo-fixit/component-projects.git themes/component-projects
```

Next edit `hugo.toml` of your project and add this theme component to your themes:

```toml
theme = ["FixIt", "component-projects"]
```

## Inject Partial

Finally, inject the theme component's assets in `layouts/partials/custom.html` within the `custom-head` or `custom-assets` block:

```go-html-template
{{- define "custom-assets" -}}
  {{- partial "inject/component-projects.html" . -}}
{{- end -}}
```

## Configuration (Optional)

Obtaining repositories information relies on GitHub official API. Before starting to use it, it is recommended to generate personal access token on GitHub to prevent GitHub API usage limit.

1. Click to jump to GitHub [Generate token](https://github.com/settings/tokens/new), select the scope named `public_repo` to generate personal access token.
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

### Content Adapter

:tada: This is a awesome feature! It can generate blog posts from the README of the repositories according to the projects data you configured.

You can copy the [content adapter template](/_content.gotmpl) of this component to your project:

```plain
content/
├── projects/
│   ├── _content.gotmpl  <-- content adapter
│   └── _index.md
data/
└── projects.yml         <-- project data
```

Then, open the `hugo.toml` file and configure the `projectsAdapters` option to enable the content adapter:

```toml
[params]
  [params.projectsAdapters]
    enable = true
    onlyPublic = true
    categories = []
    collections = []
    ignoreList = []
```

### Custom Blocks

You can implement these blocks through `define`.

| Block Name        | Description                                       |
| :---------------- | :------------------------------------------------ |
| `projects-aside`  | Displayed in the aside of the projects page       |
| `projects-meta`   | Displayed in the post meta of the projects page   |
| `projects-footer` | Displayed in the post footer of the projects page |

## Scheduled tasks

Since it uses server-side rendering, all data is fetched at build time and not requested from the GitHub API on each visit. Therefore, we can use scheduled tasks to update the data to keep it up to date.

### Deploy to GitHub Pages

If your site is hosted on GitHub Pages, you can use GitHub Actions to deploy automatically.

```yaml
name: Hugo build and deploy
on:
  schedule:
    # Rebuid the site every day at 00:00 UTC to update the projects data
    - cron: '0 0 * * *'
  push:
    branches: [ main ]
  workflow_dispatch:
jobs:
  # Your build and deploy jobs here
```

### Deplot to Vercel

If your site is hosted on Vercel, you can use Vercel's [Deploy Hooks](https://vercel.com/docs/deployments/deploy-hooks#creating-&-triggering-deploy-hooks) feature with GitHub Actions to deploy automatically.

```yaml
name: Vercel deploy hook
on:
  schedule:
    # Rebuid the site every day at 00:00 UTC to update the projects data
    - cron: '0 0 * * *'
jobs:
  Vercel-Deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Vercel deploy hook
        run: |
          curl -X POST ${{ secrets.VERCEL_DEPLOY_HOOK }}
```

Create a deploy hook in the project settings of Vercel and add the `VERCEL_DEPLOY_HOOK` variable in the Secrets of the GitHub project.

## Troubleshooting

You can add the `--ignoreCache` parameter to the `hugo server` command to clear the cache in local server.
