# Mathematics-based Web APIs Documentation

This repository hosts the source files for the **Mathematics-based Web APIs Documentation Site**, built with [MkDocs](https://www.mkdocs.org/) and [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

## 📖 Overview

The site introduces the concept of offering **mathematical problem-solving and algorithms as web-based APIs**. It demonstrates how modern documentation frameworks can make these APIs accessible, understandable, and developer-friendly.

## 🚀 Live Site

The documentation is automatically deployed to [Vercel](https://vercel.com/) using **GitHub Actions**.

## 🛠 Local Development

To preview the site locally:

```bash
# 1. Clone this repo
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. Create a virtual environment
python -m venv .venv
source .venv/bin/activate   # Linux/Mac
.venv\Scripts\activate      # Windows

# 3. Install dependencies
pip install -r requirements.txt

# 4. Serve the site locally
mkdocs serve
```

The site will be available at [http://127.0.0.1:8000](http://127.0.0.1:8000).

## 📦 Deployment

Deployment is handled by **GitHub Actions**.  
Every push to the `main` branch triggers a build and publishes the site automatically to Vercel.

---

Authored by **Athar Kharal, PhD**
