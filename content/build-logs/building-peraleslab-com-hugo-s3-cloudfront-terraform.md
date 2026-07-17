---
title: "Building peraleslab.com: Hugo + S3 + CloudFront + Terraform"
description: "This site is a working example of its subject matter. A Hugo static site hosted on S3 and delivered by CloudFront. TLS via ACM with Cloudflare DNS. Deployed on every push with GitHub Actions using OIDC — no stored credentials. The entire stack is fully defined in Terraform."
date: 2026-07-15
tag: "Build Log"
featured: true
draft: false
---

Lorem ipsum dolor sit amet, consectetur adipiscing elit. This site is a working
example of its own subject matter: a Hugo static site hosted on S3 and
delivered by CloudFront, with TLS handled by ACM and DNS managed in Cloudflare.

Every push to `main` builds the site with the pinned Hugo version, syncs the
output to S3, and invalidates the CloudFront distribution — all authenticated
via GitHub's OIDC provider assuming a scoped IAM role. No long-lived
credentials are stored anywhere in the pipeline.

Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. The entire
stack — S3 bucket, CloudFront distribution, ACM certificate, and the IAM OIDC
role — is fully defined in Terraform in a separate infrastructure repository.
