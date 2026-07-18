---
title: "Building peraleslab.com: Hugo + S3 + CloudFront + Terraform"
description: "This site is a working example of its subject matter. A Hugo static site hosted on S3 and delivered by CloudFront. TLS via ACM with Cloudflare DNS. Deployed on every push with GitHub Actions using OIDC — no stored credentials. The entire stack is fully defined in Terraform."
date: 2026-07-15
tag: "Build Log"
featured: true
draft: false
---

## Why build this myself?

A conversation I've encountered repeatedly throughout my career goes
something like this: someone asks how a system is deployed, how a
particular part of the environment works, or why something was designed a
certain way — and the honest answer is, "Let me check the documentation. I
didn't build that part."

That is normal in enterprise IT. No one person owns every layer of a large
system.

But for peraleslab.com, I wanted something different.

I wanted at least one environment that I understood and owned end to end:
the design, the code, the infrastructure, and the deployment process. If
I'm going to write about migrations, architecture, and the tradeoffs that
come with technical decisions, it seems only natural to have a place where
I can build, test, and explore those ideas for myself.

So that is what peraleslab.com is intended to be: not just a place where I
write about technology, but a working environment where I can build, test,
change, occasionally break, and document things along the way.

## The site

I wanted something open source, relatively simple to operate, and capable
of handling much of the site-building functionality without requiring me to
assemble a collection of additional frameworks and tools. That led me to
[Hugo](https://gohugo.io/).

The site is built with Hugo, using the Extended edition, with the version
pinned in a single `.hugo-version` file that both my local environment and
the CI workflow use.

The goal is simple: the version I build locally should be the same version
that builds the production site. That removes one more source of
unnecessary drift between development and deployment.

One of the things I have come to appreciate about Hugo is how much
functionality is already built into the framework. Content management,
templates, reusable partials, SCSS processing, and static-site generation
all live within the same toolchain. For a site like this, that means I can
build what I need without introducing a separate front-end framework or a
collection of additional dependencies.

## Deployment without long-lived AWS credentials

The architecture diagram below represents the actual deployment path.

{{< diagram >}}

A push to the main branch triggers GitHub Actions. The workflow uses OpenID
Connect, or OIDC, to assume a narrowly scoped IAM role in AWS and receive
temporary credentials for the deployment.

That means there is no long-lived AWS access key stored in GitHub secrets.

The workflow then builds the site using the same pinned Hugo version I use
locally, synchronizes the generated static files to Amazon S3, and
invalidates the CloudFront distribution, so the updated content is
available through the CDN.

The public side of the architecture is intentionally simple. CloudFront
serves the static site, while AWS Certificate Manager provides the TLS
certificate. The certificate is validated through a DNS record in
Cloudflare, where I manage the domain.

There are no web servers to patch, no application runtime to maintain, and
no public virtual machines sitting behind the site. For what is
fundamentally a collection of static content, I wanted the hosting
architecture to remain static as well.

## Separating the site from the infrastructure

The application and infrastructure also live in separate repositories.

This repository owns the site: the content, Hugo configuration, layouts,
styling, and deployment workflow.

A separate infrastructure repository owns the AWS resources supporting it,
including the S3 bucket, CloudFront distribution, ACM certificate, and the
IAM trust relationship used by GitHub Actions.

That separation was deliberate.

I wanted a clear boundary between the thing being deployed and the
infrastructure it is being deployed into. It also gives me room to evolve
the infrastructure independently without turning the site repository into a
collection of unrelated Terraform scripts.

## Layout-driven instead of manually curated

I did not want to maintain a hardcoded list of featured or recent articles
every time I published something new.

Instead, the homepage is driven by content metadata.

A post can be explicitly marked as featured through its front matter. If
nothing is marked as featured, the site falls back to the newest build-log
entry.

The Latest Articles section works the same way. Hugo automatically selects
the four most recent posts across the site's content sections and excludes
whichever post is already being displayed as the featured article.

The practical result is that publishing a new article should require
exactly one thing: publishing the article. I should not also have to
remember to update a homepage template afterward.

## What's next

This is the first build-log entry on peraleslab.com.

The intention is for the build log to become the history of the site
itself.

When I change the deployment process, add a new service, redesign part of
the architecture, or discover that one of my original decisions was a bad
idea, I want that change documented here rather than buried in Git history.

The site is a finished product in the sense that it is live.

It is very much unfinished in every other sense.
