---
title: "Building peraleslab.com: Hugo + S3 + CloudFront + Terraform"
description: "This site is a working example of its subject matter. A Hugo static site hosted on S3 and delivered by CloudFront. TLS via ACM with Cloudflare DNS. Deployed on every push with GitHub Actions using OIDC — no stored credentials. The entire stack is fully defined in Terraform."
date: 2026-08-08
tag: "Build Log"
featured: true
draft: false
---

I created peraleslab.com as both a technical writing site and a working environment where I could build, test, and document the technologies I use. It is the second site I have built using this same overall blueprint. The Digital Pensieve established the pattern, and this site reuses it with its own configuration, content, and infrastructure.

One of my goals was to own the environment end to end: the site design, application code, infrastructure, and deployment process. It gives me a place to experiment with architecture and automation while documenting what I learn along the way.

## The Site

The site is built with Hugo, with the version pinned in a `.hugo-version` file used by both my local environment and the CI workflow. This keeps the local and production builds on the same version and avoids unnecessary differences between environments.

Hugo provides the content management, templates, reusable partials, SCSS processing, and static-site generation needed for the site without requiring an additional front-end framework.

## Deployment

A push to the main branch triggers GitHub Actions. The workflow builds the Hugo site, uploads the generated static files to Amazon S3, and invalidates the CloudFront distribution.

GitHub Actions authenticates with AWS using OpenID Connect, or OIDC, and assumes a scoped IAM role using temporary credentials. No long-lived AWS access keys are stored in GitHub.

{{< diagram >}}

CloudFront provides HTTPS delivery and caching, while AWS Certificate Manager provides the TLS certificate. DNS is managed through Cloudflare.

The result is a simple static hosting architecture with no web servers, application runtime, or public virtual machines to maintain.

## Separating the Site from the Infrastructure

The site and its infrastructure are maintained in separate GitHub repositories.

The site repository contains the Hugo content, configuration, layouts, styling, and deployment workflow. A separate Terraform repository manages the AWS infrastructure supporting the site, including S3, CloudFront, ACM, and IAM.

This keeps the application and infrastructure concerns separate while allowing each to evolve independently.

## Automating the Homepage

The homepage is driven by Hugo content metadata rather than manually maintained article lists.

Posts can be marked as featured, while the latest articles are selected automatically from the site's content. Publishing a new article therefore does not require manually updating the homepage.

## What's Next

As I change the architecture, deployment process, infrastructure, or site design, I plan to document those changes here.

The site is live, but it is also intentionally a work in progress.
