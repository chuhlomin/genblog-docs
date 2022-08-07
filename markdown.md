---
template: _page
type: page
order: 10
---

# Markdown files

Markdown is a plain text [formatting syntax](https://daringfireball.net/projects/markdown/syntax).

GenBlog reads every Markdown file (`.md`) and parses it into `MarkdownFile` Go structure.

For example, if `file.md` has a content like this:

````md
# This is a header

Hello

* Red
* Green
* Blue

> This is a blockquote

```go
package main
```
````

Then `MarkdownFile` structure for this file will look like this:

```json
{
  "Source": "file.md",
  "Path": "file.html",
  "Canonical": "file.html",
  "ID": "file.md",
  "Markdown": "\nHello\n\n* Red\n* Green\n* Blue\n\n\u003e This is a blockquote\n\n```go\npackage main\n```\n",
  "Title": "This is a header",
  "Body": "\u003cp\u003eHello\u003c/p\u003e\n\n\u003cul\u003e\n\u003cli\u003eRed\u003c/li\u003e\n\u003cli\u003eGreen\u003c/li\u003e\n\u003cli\u003eBlue\u003c/li\u003e\n\u003c/ul\u003e\n\n\u003cblockquote\u003e\n\u003cp\u003eThis is a blockquote\u003c/p\u003e\n\u003c/blockquote\u003e\n\n\u003cpre\u003e\u003ccode class=\"language-go\"\u003epackage main\n\u003c/code\u003e\u003c/pre\u003e\n",
  "ContentType": "post",
  "Language": "en"
}
```

All these fields available in templates.

## MarkdownFile structure fields

Basic fields are:

| Field       | Description |
|-------------|-------------|
| Source      | Path to the original Markdown file `source_directory`. |
| Path        | Path to generated HTML file in `output_directory`. |
| Canonical   | Path to generated HTML file in `output_directory` in default language. To use in meta tag `<link rel="canonical" href="...">`. |
| ID          | Path to the corresponding Markdown file in `source_directory` in default language. Same posts in different languages will have the same ID. |
| Markdown    | Origilan file content, except metadata block (see below). Used to in search index. |
| Title       | Title, parsed from Markdown file content. |
| Body        | HTML body generated from `Markdown` field. |
| ContentType | Defaults to `"posts"`, can be overriden in the metadata block (see below). Can be used in templates. |
| Language    | Defaults to `default_language` in config. Can be overriden by the metadata block or if Markdown filename has language-specific suffic.

There are some additional fields that can be populated with information from matadata block.

## Metadata block

You can add an _optional_ "metadata block" at the beginning of every Markdown file.

It looks like a YAML, separated by "`---`" lines from the rest of the file:

```md
---
date: 2022-08-06
comments_enabled: false
---

# What a wonderful day

Innit?
```

Available metadata fields:

| Metadata field   | `MarkdownFile` field | Description |
|------------------|----------------------|-------------|
| date             | Date                 | Date of the post, used to sort posts later on (see `Data` structure in Templates) |
| title            | Tilte                | If markdown file has no header, it will be used as title. |
| comments_enabled | CommentsEnabled      | Overrides `comments_enabled` config. Can be used in templates |
| tags             | Tags                 | Comma-separated list of tags |
| language         | Language             | Overrides `default_language` and language parsed from filename |
| draft            | Draft                | Marks file as draft, it will be skipped unless `show_drafts` is enabled |
| template         | Template             | Overrides `template` config |
| order            | Order                | Used in `sort` template function |
| description      | Description          | To use in template, like in `<meta name="description" content="...">` |
| author           | Author               | To use in template, like in `<meta name="author" content="...">` |
| keywords         | Keywords             | To use in template, like in `<meta name="keywords" content="...">` |
| image            | Image                | Path or URL to promo image for the post |
