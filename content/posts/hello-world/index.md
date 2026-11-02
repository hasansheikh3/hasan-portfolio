---
title: "Hello, world"
date: 2026-11-02T00:00:00Z
draft: false
summary: "A first post, and a quick tour of how this site is put together."
tags: ["meta", "hugo"]
categories: ["Notes"]
cover:
  image: "cover.jpg"
  alt: "An abstract blue gradient"
  caption: "A generated placeholder cover — swap in your own image."
ShowToc: true
---

Welcome to the first post on this site. This page is a placeholder: replace it with
your own writing, then delete it once you have something better.

## How the site works

- **[Hugo](https://gohugo.io/)** turns Markdown files into static HTML.
- **[PaperMod](https://github.com/adityatelange/hugo-PaperMod)** provides the theme.
- **Netlify** builds the site from Git and serves it from its CDN.

There is no database and no server to maintain. A post is just a folder with an
`index.md` file, and publishing is a `git push`.

## Anatomy of a post

Each post is a *page bundle*:

```text
content/posts/hello-world/
├── index.md
└── cover.jpg
```

Keeping images next to the post means they can be referenced by filename and
processed at build time.

## Images

Raster images in a page bundle can be resized and converted to WebP automatically:

```go-html-template
{{</* responsive-img src="cover.jpg" alt="An abstract blue gradient" */>}}
```

That shortcode is defined in `layouts/_shortcodes/responsive-img.html` and uses the
partial in `layouts/_partials/responsive-img.html`. It emits a `srcset` so phones
download a smaller file than desktops.

{{< responsive-img src="cover.jpg" alt="An abstract blue gradient" >}}

You can also set a cover image in the front matter, as this post does — PaperMod
renders it responsively on the post page and in listings.

## Next steps

- Edit `hugo.toml` with your name, description, and links.
- Rewrite `content/about.md`.
- Add posts with `hugo new content posts/my-post/index.md`.
