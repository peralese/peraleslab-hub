---
title: "Home Lab and Self-Hosted Application Environment"
description: "A tour of the hardware, network, and services behind the homelab: media, internal apps, local DNS, and selectively exposed external services."
date: 2026-08-04
tag: "Home lab"
draft: false
---

This home lab provides a compact platform for hosting media services, internal applications, development and test workloads, local DNS, and selected externally accessible services.

The environment supports practical experimentation with infrastructure, application modernization, Linux hosting, DNS, secure access, and self-hosted application deployment. It also provides a controlled platform for developing, testing, and operating personal software projects.

## Network and Connectivity

The environment begins with an AT&T internet gateway connected to a TP-Link Deco XE75 Pro mesh system, which provides routing and wireless connectivity throughout the home.

A managed multi-gigabit switch serves as the wired backbone for the lab. It connects the servers and provides capacity for future storage systems and higher-speed network devices.

At a high level, the network supports three primary compute platforms:

* A Plex and application server
* A Mac mini used for shared infrastructure and personal applications
* A Linux server intended to host the redesigned TravelPets.com website

## Plex and Application Server

The Plex server is the primary media platform in the environment. It runs Plex Media Server and provides access to the associated media library.

It also hosts a local test instance of TravelPets.com. This instance was created from a copy of the currently hosted website and migrated into the home lab.

The existing TravelPets application runs on Windows, IIS, Classic ASP, and Microsoft SQL Server. Hosting a local copy allows changes to be developed and evaluated without affecting the publicly available website.

## Mac mini Application Server

The Mac mini provides shared infrastructure services and hosts several personal applications.

Its infrastructure responsibilities include:

* Local DNS resolution
* DNS-based filtering
* Network-level ad blocking
* Name resolution for internal applications

AdGuard Home provides these services and allows internal applications to be accessed through readable hostnames rather than IP addresses and port numbers.

The Mac mini also hosts several small applications I created for personal use:

* A personal knowledge-base experiment
* A catalog for tracking movies in the Plex library
* A weekly meal-planning application
* A home-project tracking application
* A task-tracking application

Most of these applications are available only from the internal network. The task-tracking application is also available externally through Cloudflare Tunnel.

## Linux Web Server

A separate Linux mini-PC is being prepared to host the redesigned version of TravelPets.com.

The long-term objective is to move the website away from its existing third-party Windows hosting environment and operate the modernized application locally.

The current platform is based on:

* Windows hosting
* IIS
* Classic ASP
* Microsoft SQL Server

The redesigned platform is being built with:

* Linux
* PostgreSQL
* Django
* A modernized web application architecture

The migration and modernization effort is a summer project I assigned to my son, who is studying software engineering. I am providing architectural guidance while he works through the application design, data migration, development, testing, and deployment process.

That effort will be covered in a separate post because it involves more than a platform migration. It also includes application modernization, database conversion, and the practical challenges of replacing a long-running legacy website.

## External DNS and Secure Access

The environment uses separate approaches for public and internal name resolution.

Amazon Route 53 provides public authoritative DNS for selected services associated with the lab domain. It resolves approved public application names to their external access paths.

Cloudflare Tunnel provides secure external access to selected applications without requiring direct inbound port forwarding to the home network. This allows individual services to be exposed selectively while the broader environment remains private.

AdGuard Home handles internal DNS and resolves private application names for devices connected to the home network.

## Architectural Approach

The environment is best described as a small hybrid home lab and self-hosted application platform.

It combines local infrastructure with selected cloud services:

* Local servers provide compute, storage, DNS, and application hosting.
* Route 53 provides public authoritative DNS.
* Cloudflare Tunnel provides secure external connectivity.
* AdGuard Home provides internal DNS resolution and filtering.
* Separate servers provide a degree of workload isolation between media services, shared applications, and the future production website.

The environment provides a practical platform for exploring architectural concerns that also appear in larger environments, including workload placement, internal and external DNS, application modernization, secure access, platform selection, service isolation, and operational support.
