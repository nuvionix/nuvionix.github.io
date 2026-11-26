---
title: "From Zero to Hugo: Build and Deploy a Blog"
date: 2026-11-26
draft: false
description: "Create a static Hugo site, add content, preview it, and deploy it using GitHub Actions."
categories: ["Hugo"]
tags: ["Hugo"]
readingTime: true
---

## Why Hugo?

Hugo is a static-site generator. It turns content files, templates, and configuration into HTML, CSS, and other static assets. Because the published site does not need a server-side application for each page request, it can be hosted on services such as GitHub Pages.

## Requirements

You need Git, Hugo (the extended edition is recommended for themes that use Sass), a GitHub account, and basic Markdown knowledge. Check the current Hugo installation instructions for your operating system and verify the installation with `hugo version`.

## Create a site

```bash
hugo new site my-blog
cd my-blog
```

A Hugo project contains directories such as `content`, `layouts`, `static`, `archetypes`, and configuration. The exact structure depends on the theme and project conventions.

## Add content

Create a Markdown page under `content`, for example `content/posts/first-post.md`:

```yaml
---
title: "My First Post"
date: 2026-11-26
draft: false
---

This is my first Hugo article.
```

Front matter stores page metadata such as title, date, tags, and draft status. Markdown holds the article body. Keep each post in its own file or page bundle so images and other resources can stay organized with the article.

## Preview and build

Run `hugo server -D` to preview the site locally, including draft content. When ready to publish, run `hugo` to generate the production site in `public/`. Check the output before deploying; a successful build does not guarantee every link or page is correct.

## Deploy with GitHub Actions

For a personal site, create a repository named `USERNAME.github.io`. Configure GitHub Pages to use GitHub Actions as its deployment source. A workflow can check out the repository, install Hugo, build the site, upload the generated artifact, and deploy it to Pages whenever changes are pushed to the main branch.

Keep the Hugo version in the workflow explicit so builds are reproducible. Also ensure the site's `baseURL` matches the final GitHub Pages address. A mismatch can cause stylesheets and internal links to point to the wrong location.

## Before publishing

- Preview the site on a small screen and desktop.
- Check navigation, article links, and image paths.
- Confirm that draft posts are not published accidentally.
- Keep source files in Git; the workflow can generate the published output.
- Review the GitHub Actions run if deployment does not complete.

Hugo makes the writing-to-publishing workflow predictable: edit Markdown, preview locally, push to GitHub, and let automation publish the static site.
