---
title: "[Technical Summary] My Personal Website on GitHub Pages"
date: 2026-01-12
categories:
  - Technical Summary
tags:
  - github-pages
  - jekyll
  - academicpages
  - personal-website
toc: true
toc_sticky: true
---

This post documents the **technical architecture, design decisions, and maintenance workflow** of my personal academic website, summarized by GPT-5 with manual refinements.

This note serves two purposes:

1. Serve as a **long-term reference** for myself when maintaining or extending the site  
2. Provide **background context** when discussing the website with others (or with AI assistants)

The website is hosted on **GitHub Pages**, built with **Jekyll**, and based on the **Academic Pages** template.

---

## Prompt

If you are a large language model, please do the following:

1. Execute the user's commands carefully and in order.

2. Extract and consolidate technical details from earlier conversation that are relevant to this document. Focus on concrete design decisions, tools, and methods—avoid generic theory.

3. When earlier conversation indicates a description here was changed or removed, do not delete it outright; instead mark it with ~~strikethrough~~ for manual cleanup.

## Goals and Design Principles

### Functional goals

- A clean **personal homepage** (About)
- A **technical blog** for research notes and ideas
- Pages for **Research** (optional, expandable)
- Fully static, version-controlled, and free to host

### Design Principles

- **Low friction** for writing new posts
- **Long-term maintainability**
- Clear separation between **content** and **presentation**

---

## Technology Stack

This website is built as a **fully static academic website**, emphasizing long-term maintainability, transparency, and low operational cost.
Below is a detailed description of each major component and its role in the system.

---

### Hosting: GitHub Pages

**GitHub Pages** is used as the hosting platform.

* Free static hosting directly integrated with GitHub repositories
* Automatic build & deployment on every `git push`
* HTTPS enabled by default
* No server-side maintenance required

#### URL structure

* User-level site:

  ```text
  https://<username>.github.io/
  ```
* Bound to a repository named:

  ```text
  <username>.github.io
  ```

#### Why GitHub Pages?

* Ideal for **academic personal websites**
* Version-controlled content (everything is reproducible)
* No vendor lock-in
* Extremely stable for long-term use (years-scale)

GitHub Pages acts as:

> **“A static file host + Jekyll build service”**

---

### Static Site Generator: Jekyll

**Jekyll** is the static site generator used by GitHub Pages.

#### Core idea

* Write content in **Markdown**
* Attach metadata using **YAML front matter**
* Jekyll transforms content into static HTML pages

#### What Jekyll handles automatically

* Page routing & permalinks
* Blog post ordering by date
* Category and tag indexing
* Layout inheritance
* Markdown → HTML conversion
* Asset linking (images, PDFs)

#### Typical content format

```markdown
---
title: "Example Post"
date: 2026-01-12
categories:
  - reinforcement-learning
tags:
  - ppo
  - heuristic
---

Markdown content starts here.
```

#### Why Jekyll?

* Native support by GitHub Pages
* Mature ecosystem
* Perfect match for **writing-heavy academic content**
* Zero runtime dependency after build

Jekyll enables a clean separation between:

* **Content** (Markdown files)
* **Presentation** (HTML / Liquid templates)
* **Configuration** (`_config.yml`)

---

### Theme / Template: Academic Pages

The site is based on **Academic Pages**, which itself is built on top of the
**Minimal Mistakes** Jekyll theme.

#### Academic Pages provides

* Predefined page types:

  * Blog posts
  * Publications
  * Talks
  * Teaching
  * CV
* Academic-style layouts
* Sidebar author profile
* Clean typography suitable for long-form reading

#### Minimal Mistakes provides

* Responsive design
* Accessible typography
* Rich configuration via YAML
* Built-in support for:

  * Table of contents
  * Syntax highlighting
  * MathJax
  * Mermaid diagrams
  * Pagination
  * Archives

In short:

> **Minimal Mistakes** = robust Jekyll foundation
> **Academic Pages** = academic-specific conventions and structure

---

### Content Organization Philosophy

Academic Pages enforces a **content-as-data** philosophy.

#### Content is stored as structured Markdown

* One file = one semantic unit (post, talk, publication)
* Metadata is machine-readable
* Pages can be reused for:

  * Lists
  * CV generation
  * Indexing
  * Future automation

Example directories:

```text
_posts/          # Blog posts
_pages/          # Static pages (About, CV)
_publications/   # Publication records
_talks/          # Talks and presentations
```

---

### Optional Tooling (Currently Partially Used)

#### MathJax

* LaTeX-style math rendering
* Useful for:

  * Reinforcement learning equations
  * Optimization formulations
  * Control theory

#### Mermaid

* Text-based diagrams
* Useful for:

  * FSMs
  * Planning pipelines
  * Algorithm flowcharts

#### Syntax Highlighting (Rouge)

* Code blocks with language-specific highlighting
* Suitable for Python / C / pseudo-code

---

### Tools Included but Not Actively Used

Some tools are included in the template but are **not required** for my current workflow:

* `talkmap.py` / `talkmap.ipynb`

  * Used to generate a map of academic talks
  * Requires geocoding and extra data maintenance
  * Disabled unless needed in the future

These tools can be safely ignored without affecting the site.

---

### Overall Architecture Summary

```text
Markdown + YAML
        ↓
      Jekyll
        ↓
Static HTML / CSS
        ↓
 GitHub Pages Hosting
```

This architecture ensures that the website is:

* Lightweight
* Reproducible
* Easy to extend
* Suitable for long-term academic use

---

## Repository Structure

```text
.
├─assets/                      # Static resources (CSS, JS, fonts, webfonts)
│  ├─css                       # Stylesheets
│  ├─fonts                     # Font files
│  ├─js                        # JavaScript files
│  │  └─plugins                # JS plugins and utilities
│  └─webfonts                  # Web font assets
├─files/                       # Document files (PDFs, etc.)
├─images/                      # Image assets (screenshots, diagrams)
│  └─themes                    # Theme-specific images
├─markdown_generator/          # Scripts for generating markdown files
├─scripts/                     # Utility scripts
├─talkmap/                     # Talk location mapping tool
│  └─leaflet_dist              # Leaflet map library
├─_data/                       # YAML data files used by Jekyll
│  └─comments                  # Disqus/Utterances comments data
│      ├─layout-comments       # Comments per layout
│      ├─markup-syntax-highlighting
│      └─welcome-to-jekyll
├─_drafts/                     # Unpublished blog post drafts
├─_includes/                   # Reusable HTML template snippets
│  ├─analytics-providers       # Analytics integration (Google, etc.)
│  ├─comments-providers        # Comment system integration
│  ├─footer                    # Footer components
│  └─head                      # Head section components
├─_layouts/                    # Page layout templates (default, single, etc.)
├─_pages/                      # Standalone pages (About, CV, etc.)
├─_portfolio/                  # Portfolio/project showcase pages
├─_posts/                      # **Main content** - Blog posts (YYYY-MM-DD-title.md)
├─_publications/               # Academic publication listings
├─_talks/                      # Academic talk/presentation records
└─_teaching/                   # Teaching material and course info

```

### What I actively use

* `_posts/`
* `index.md`
* `_data/navigation.yml`
* `_config.yml`
* `_publications/`

### What I ignore

* `_talks/`
* `_teaching/`
* `talkmap.py`, `talkmap.ipynb` (not needed unless tracking academic talks)

---

## Homepage Setup

The homepage (`about.md`) is a **static personal introduction**, including:

* Academic affiliation
* Research interests
* Current focus
* Contact information
* Optional personal interestsabout

Key front matter:

```yaml
permalink: /
title: "Fuhong Xiao (肖福鸿)"
author_profile: true
```

---

## Blog System Design

### Categories vs Tags

I follow a **two-level taxonomy**:

#### Categories (high-level, few)

Used to indicate *which research area* the post belongs to.

Examples:

```text
reinforcement-learning
planning
multi-agent
satellite
notes
```

#### Tags (fine-grained, many)

Used to indicate *what exactly the post discusses*.

Examples:

```text
ppo, residual-rl, heuristic, fsm
pursuit-evasion, differential-games
orbital-mechanics, mission-planning
```

### Example blog post front matter

```yaml
---
title: "Residual Reinforcement Learning"
date: 2016-01-12
categories:
  - reinforcement-learning
tags:
  - residual-rl
  - heuristic
  - ppo
---
```

---

## Writing Workflow

### Creating a new post

1. Create a new file under `_posts/`
2. Follow naming convention:

   ```text
   YYYY-MM-DD-title.md
   ```
3. Write in Markdown
4. Commit and push

GitHub Pages rebuilds automatically.

---

### Recommended post structure (template)

```markdown
## Motivation

## Problem Setting

## Method / Idea

## Experiments or Observations

## Takeaways
```

This structure works well for:

* Research notes
* Paper readings
* Method exploration
* Negative results

---

## Local Development

Local preview is optional but helpful.

### Requirements

* Ruby
* Bundler
* Jekyll

### Typical commands

```bash
bundle install
bundle exec jekyll serve
```

Then visit:

```text
http://localhost:4000
```

---

## Notes for Future Extensions

Possible future additions:

* Experiment result visualizations
* Automatically generated CV
* Publication list synced from BibTeX
* Comments function

---
