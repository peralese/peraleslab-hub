# peraleslab.com

Source for [peraleslab.com](https://peraleslab.com) — a Hugo static site with a
custom theme (no third-party theme), deployed to S3 + CloudFront via GitHub
Actions using OIDC.

## Requirements

- **Hugo extended**, pinned to the exact version in [`.hugo-version`](.hugo-version).
  This file is the single source of truth: local dev and the CI workflow
  ([`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)) both read it.

  ```
  brew install hugo   # or your platform's equivalent
  hugo version        # confirm it matches .hugo-version
  ```

## Local development

```
hugo server -D
```

`-D` includes draft content. Omit it to preview exactly what will ship.

## Content

Content lives under `content/<section>/`, one section per nav item:

- `build-logs/` — includes IaC/Terraform content; not a separate section
- `migration/`
- `architecture/`
- `lab-notes/`
- `tools/`
- `ai/`
- `about.md` (standalone page, not a section)

Front matter fields used by the templates:

| Field         | Purpose                                                              |
| ------------- | --------------------------------------------------------------------|
| `title`       | Post title                                                           |
| `description` | One-line summary shown in article rows and the featured card         |
| `date`        | Publish date; drives sort order for "Latest articles"                |
| `tag`         | Single display tag shown as a pill (independent of the section slug) |
| `featured`    | `true` on at most one build-log post to pin it as the featured card  |
| `draft`       | Must be `false` to appear on the site                                |

The homepage (`layouts/index.html`) is layout-driven:

- **Featured build log** — the post with `featured: true` (across all
  sections), falling back to the newest post in `build-logs/` if none is set.
- **Latest articles** — the four most recent posts across all sections,
  excluding whichever post is currently featured.

Create a new post with:

```
hugo new content/build-logs/my-new-post.md
```

## Theme

No third-party theme — layouts and styling are custom and live directly in
this repo:

- `layouts/` — templates and partials (base layout, homepage, single/list
  views, the inline SVG architecture diagram, header/footer)
- `assets/scss/main.scss` — the only stylesheet, compiled via Hugo Pipes
  (embedded LibSass). Plain CSS, dark theme only, single blue accent, no CSS
  or JS framework.

## Deploy

`.github/workflows/deploy.yml` runs on every push to `main`:

1. Builds with the pinned Hugo version.
2. Assumes `arn:aws:iam::936453552758:role/peraleslab-com-deploy` via GitHub's
   OIDC provider — no stored AWS credentials.
3. `aws s3 sync`s `public/` to `peraleslab.com-936453552758-us-east-1-hb`.
4. Invalidates CloudFront distribution `E12YFEKOCUMQHY`.

Hugo's default `/section/post-slug/` output is used as-is; pretty URLs are
handled by an existing CloudFront Function (outside this repo).

## Out of scope

Terraform/infrastructure, DNS (Cloudflare), and the Pensieve blog
(`pensieve.peraleslab.com`) live in their own repos and are not touched here.
