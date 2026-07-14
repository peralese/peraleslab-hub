---
title: "OIDC Deploy Pipelines with GitHub Actions and AWS"
description: "Use GitHub's OIDC provider to assume roles in AWS securely — no long-lived credentials."
date: 2025-04-28
tag: "CI/CD"
draft: false
---

Lorem ipsum dolor sit amet, consectetur adipiscing elit. GitHub Actions can
assume an AWS IAM role directly via its built-in OIDC identity provider,
removing the need to store long-lived AWS access keys as repository secrets.

Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
aliquip ex ea commodo consequat. This post walks through configuring the
trust policy, scoping the role's permissions, and wiring the workflow.
