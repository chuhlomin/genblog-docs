---
template: _page
type: index
order: 0
---

# GenBlog

GenBlog is a free and [open-source](https://github.com/chuhlomin/genblog) static site generator written in Go.
It parses provided [Markdown files](markdown.md) and passes relevant information to [Go-templates](templates.md).

You may use a GitHub Action to run GenBlog in GitHub Actions:

```yaml
name: main

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  main:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: GenBlog
        uses: chuhlomin/genblog@v1.0-rc5
        with:
          base_path: https://mydomain.com/
          static_directory: _static

      - name: Setup Pages
        uses: actions/configure-pages@v1

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: output

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@main
```

## Config

| GitHub Action input     | Default valie                 | Description                                                                                                         |
|-------------------------|-------------------------------|---------------------------------------------------------------------------------------------------------------------|
| base_path               | `""`                          | Beginning of the generated URLs.                                                                                    |
| source_directory        | `"."`                         | Path to directory with Markdown files.                                                                              |
| output_directory        | `"output"`                    | Path to write files to.                                                                                             |
| static_directory        | `""`                          | If set, all files from this directory will be copied to `output_directory` as-is. Keep your CSS, JS and fonts there |
| allowed_file_extensions | `".jpeg,.jpg,.png,.mp4,.pdf"` | Comma-separated list of allowed file extensions that will be copied to `output_directory`.                          |
| templates_directory     | `"_templates"`                | Path to templates directory                                                                                         |
| default_template        | `"_post.html"`                | Filename of the default template                                                                                    |
| default_language        | `"en"`                        | Default language                                                                                                    |
| comments_enabled        | `true`                        | Enable comments, can be overrdidden in Markdown files metadata block.                                               |
| comments_site_id        | `""`                          | ID of the site in comments system. Only used in templates.                                                          |
| show_drafts             | `false`                       | Process drafts. Drafts are Markdown files with `draft: true` metadata block.                                        |
| thumb_path              | `"thumb"`                     | Path to directory with thumbnails in `output_directory`.                                                            |
| thumb_max_width         | `"140"`                       | Max width of thumbnails.                                                                                            |
| thumb_max_height        | `"140"`                       | Max height of thumbnails.                                                                                           |
| search_enabled          | `false`                       | Enable search.                                                                                                      |
| search_url              | `""`                          | URL of the search service. Only used in templates.                                                                  |
| search_path             | `"search_index"`              | Path to search index directory (different from `output_directory`).                                                 |

Most of string values can be accessed in templates using `config` function.
For example, `{{ config "BasePath" }}` will return `base_path` value.
