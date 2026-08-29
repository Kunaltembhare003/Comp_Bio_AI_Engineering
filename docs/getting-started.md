# Getting Started

## Prerequisites

Before you begin, make sure you have:

- Python 3.10 or newer
- Git installed
- A GitHub repository configured for GitHub Pages

## Install dependencies

```bash
python3 -m pip install -r requirements.txt
```

## Run the documentation locally

```bash
mkdocs serve
```

This starts a local development site with live reload.

## Build the static site

```bash
mkdocs build
```

The generated files are placed in the `site/` directory.

## Deploy to GitHub Pages

This repository includes a deployment workflow in `.github/workflows/deploy-docs.yml`.

When changes are pushed to the `main` branch, GitHub Actions builds the site and publishes it to the `gh-pages` branch for GitHub Pages hosting.
