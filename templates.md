---
template: _page
type: page
order: 20
---

# Templates

GenBlog reads all templates files from the `templates_directory`.  
It requires a `default_template` template to be present (`_post.html` by default).

## Go templates

Comment:

```
{{ /* a comment inside template */ }}
```

Condition:

```
{{ if .condition }}
  {{ /* condition is true */ }}
{{ else }}
  {{ /* condition is false */ }}
{{ end }}
```

Iterator:

```
{{ range .Items }}
  {{ . }}
{{ end }}
```

## Template files

Any file in `templates_directory` which filename does not start with
underscore (`_`) will be executed into the `output_directory`.

If template filename starts with underscore (`_`) then it can be used
as named templates that can be invoked using a template control structure.
For example, if `_panel.html` looks like this:

```
{{ define "_panel" }}
  <div class="panel">
    {{ . }}
  </div>
{{ end }}
```

Then it can be used like this in any other template:

```
{{ template "_panel" . }}
```

Similarly, named templates can be referenced in Markdown files metadata block
to override `default_template` just for one file:

```md
---
template: _page
---

# About

...
```

## Template functions

GenBlog provides some function to use in templates:

| Function(Argument): Return                   | Description | Example |
|----------------------------------------------|-------------|---------|
| debugJSON(Any): string                       | Encodes passed argument into JSON and returns a string. | `{{ debugJSON . }}` |
| join([]string): string                       | Joins all strings in the array into a single string using the separator. | `{{ join .Tags "," }}` |
| bool(*bool): bool                            | Safe way to get a boolean value from a pointer. | `{{ if bool .CommentsEnabled }}...{{ end }}`. |
| nextPage(Data): MarkdownFile                 | Returns next post in the same language. | `{{ $next := nextPage . }}` |
| prevPage(Data): MarkdownFile                 | Returns previous post in the same language. | `{{ $prev := prevPage . }}` |
| allLanguageVariations(Data): []MarkdownFile  | Returns all versions of the current post in different languages. | `{{ $all := allLanguageVariations . }}` |
| langGetParameter(string): string             | Returns language GET parameter if passed string has language-specific suffix and its different from `default_language`. | `<a href="index.html{{ langGetParameter .Path }}">Home</a>` |
| langToGetParameter(string): string           | Replace lang suffix with .html and append ?lang=ru, e.g. `index_ru.html` -> `index.html?lang=ru` | `<a href="{{ langToGetParameter .Path }}">{{ .Title }}</a>` |
| year(string): string                         | Returns year from the date string. | `{{ year .Date }}` |
| i18n(string, string): string                 | Returns translated string. | `{{ i18n "Hello" .Language }}` |
| stripTags(string): string                    | Removes HTML tags from the string. | `{{ stripTags .Title }}` |
| config(string): string                       | Returns config value. | `{{ config "BasePath" }}` |
| sort([]MarkdownFile, string): []MarkdownFile | Sorts Markdownfile by `date` or `order` | `{{ range sort .All "order" }}...{{ end }}` |
