# Captivi Website

Captivi’s marketing website built with Hugo and Bookshop.

![Site screenshot](static/images/_screenshot.png)

## Features

- Pre-built pages and components
- Blog with pagination and categories
- Configurable navigation and footer
- Multiple hero options

## Tech Stack

- Hugo for static site generation
- Bookshop for component-driven content
- Node.js scripts for local dev tooling

## Prerequisites

- Hugo: install from https://gohugo.io/getting-started/installing/ (e.g., `brew install hugo`)
- Go: install from https://go.dev/ (e.g., `brew install go`)
- Node.js + npm: https://nodejs.org/

## Quickstart

1. Install dependencies: `npm i`
2. Start site + Bookshop browser: `npm run dev`
   - Bookshop: http://localhost:30775/
   - Site: http://localhost:1313/
3. Site only: `npm run start`

## Build

- Production build to `public/`: `npm run build`

## Editing Content

- Pages and posts: edit files in `content/`
- Site metadata: `data/meta.yaml`
- Navigation: `data/nav.yaml`
- Footer: `data/footer.yaml`
- Static assets: `static/`

## Deployment (GitHub Pages)

We deploy with GitHub Actions to GitHub Pages. The workflow builds the site with Hugo and publishes it using Pages.

1. In your repository settings, enable GitHub Pages with "GitHub Actions" as the source.
2. Add the workflow below to `.github/workflows/deploy.yml`.

```
name: Deploy site

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '20'
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: '0.128.1'
          extended: true
      - name: Install deps
        run: npm ci
      - name: Build
        run: npm run build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

After the first successful run, the site will be available at the configured GitHub Pages URL.
