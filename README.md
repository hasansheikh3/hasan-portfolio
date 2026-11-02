# hasan-portfolio

A personal blog and portfolio built with [Hugo](https://gohugo.io/) and
[PaperMod](https://github.com/adityatelange/hugo-PaperMod), deployed on Netlify.

Everything is static: content is Markdown in this repository, and a push triggers
a build.

## Requirements

- Hugo **extended** v0.158 or newer (PaperMod's minimum)
- Git

Check your version:

```sh
hugo version
```

## Local development

```sh
# First clone only: pull in the theme submodule
git submodule update --init --recursive

# Start the dev server with drafts
hugo server -D
```

The site is served at <http://localhost:1313/>.

Build exactly like Netlify does:

```sh
hugo --gc --minify
```

The output lands in `public/` and is intentionally gitignored.

## Writing a post

Posts are *page bundles*: one folder per post, with an `index.md` and any images.

```sh
hugo new content posts/my-new-post/index.md
```

That creates `content/posts/my-new-post/index.md` from `archetypes/default.md`.
Set `draft = false` when it is ready to publish.

Recommended structure:

```text
content/posts/my-new-post/
├── index.md
├── cover.jpg
└── diagram.png
```

## Images

Keep images inside the post's folder (or in `assets/`) — **never** in `static/`,
because everything in `static/` is published at full size and cannot be processed
by Hugo.

### Inline images

Use the bundled shortcode:

```go-html-template
{{</* responsive-img src="diagram.png" alt="How the pieces fit together" */>}}
```

It resizes the source to 480/768/1200px WebP variants, emits a `srcset`, and adds
`width`/`height` to avoid layout shift. Small images are not upscaled.

The implementation lives in:

- `layouts/_shortcodes/responsive-img.html`
- `layouts/_partials/responsive-img.html`

It only handles raster formats (JPEG, PNG, WebP, TIFF, BMP). Use a plain
`<img>` for SVG.

### Cover images

Add to the front matter and PaperMod will render a responsive cover:

```yaml
cover:
  image: "cover.jpg"
  alt: "Description of the image"
  caption: "Optional caption"
```

### Guidelines

- Export source images at about 1600–2000px on the long edge.
- Prefer JPEG/WebP at quality ~80 for photos, PNG for screenshots.
- Delete unused images; they still get copied into the build.
- Netlify Free has roughly 100 GB of bandwidth per month. Hugo's build-time
  resizing keeps that budget comfortable, but do not ship original 8 MB photos.

## Configuration

- `hugo.toml` — site title, description, menus, social links, PaperMod options.
- `netlify.toml` — build command and pinned Hugo version.
- `assets/css/extended/custom.css` — your CSS overrides.
- `content/about.md` — the About page.

Before going live, change `baseURL` in `hugo.toml` to your real domain.

Social icons are commented out near the bottom of `hugo.toml`; uncomment and fill
in your handles.

## Deploying to Netlify

1. Create an empty repository on GitHub and push this project:

   ```sh
   git remote add origin git@github.com:YOUR-USER/YOUR-REPO.git
   git push -u origin main
   ```

2. In Netlify: **Add new site → Import an existing project → GitHub**, then pick
   the repository.
3. Netlify reads `netlify.toml`, so the build command, publish directory, and
   Hugo version are already set. Confirm and deploy.
4. Add your custom domain under **Domain management**. Netlify provisions a
   Let's Encrypt certificate automatically.
5. Update `baseURL` in `hugo.toml` to the final HTTPS URL and push.

The theme is a Git submodule, so Netlify must be able to clone it. The build
command explicitly runs `git submodule update --init --recursive` to make that
reliable.

## Updating the theme

```sh
git submodule update --remote --merge themes/PaperMod
hugo --gc --minify
git add themes/PaperMod
git commit -m "Update PaperMod"
```

Pinned theme commits keep builds reproducible.

## Optional: browser-based editing

If you later want to write from a phone or browser without touching Git, add
[Sveltia CMS](https://github.com/sveltia/sveltia-cms): a single `admin/` page plus
a config file, authenticated with GitHub OAuth. It commits Markdown to this
repository, so the rest of the workflow stays the same.

## When you outgrow the free tier

Netlify Free is fine for a text blog with optimized images. If bandwidth becomes
the limiting factor:

- **Cloudflare Pages** has no bandwidth cap on its free plan.
- **Cloudflare R2** offers 10 GB of storage and zero egress fees for images.

Both are drop-in changes: the site is already fully static.
