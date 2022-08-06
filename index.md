---
template: _page
type: page
order: 0
---

# About

GenBlog is a free and open-source static site generator written in Go.
It parses provided [Markdown files](markdown.md) and passes relevant information to [Go-templates](templates.md).

You may use a GitHub Action to run GenBlog in GitHub Actions:

```yaml
name: main

on:
  push:
    branches:
      - main

jobs:
  main:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: GenBlog
        uses: chuhlomin/genblog@v1.0-rc1
        with:
          base_path: "https://mydomain.com/"
          source_directory: "."
          output_directory: "output"
          comments_enabled: "true"
          comments_site_id: "mysite"
```
