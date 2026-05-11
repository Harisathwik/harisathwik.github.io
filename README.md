# al-folio Portfolio Template + Claude Skill

**A clean, production-ready portfolio for AI/ML engineers — built on [al-folio](https://github.com/alshedivat/al-folio), customised with Claude Code.**

> Clone it. Run the Claude skill. Answer ~20 questions. Push to GitHub Pages. Done.

[![Deploy](https://github.com/AyanArshad02/ayanarshad02.github.io/actions/workflows/deploy.yml/badge.svg)](https://github.com/AyanArshad02/ayanarshad02.github.io/actions/workflows/deploy.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![al-folio](https://img.shields.io/badge/built%20on-al--folio-blue)](https://github.com/alshedivat/al-folio)

**[Live demo →](https://ayanarshad02.github.io)**

---

## What you get

A fully configured Jekyll portfolio with 5 sections:

| Page | What it does |
|------|-------------|
| **About** | Bio, profile photo, social links, your pitch |
| **Blog** | Auto-pulls articles from Dev.to via RSS — clean minimal list layout |
| **Case Studies** | Deep-dive project pages with problem / decisions / results structure |
| **Repositories** | Clean GitHub repo cards with descriptions and tech tags |
| **CV** | Full CV rendered from `resume.json` (JSONResume format) with PDF download |

---

## Quick start

### Option A — Use the Claude skill (recommended)

The Claude skill interviews you and fills in every config file automatically.

```bash
# 1. Clone this repo
git clone https://github.com/AyanArshad02/ayanarshad02.github.io.git my-portfolio
cd my-portfolio

# 2. Open Claude Code
claude

# 3. Run the skill
/build-portfolio
```

The agent will ask you for:
- Your name, email, phone, location
- GitHub, LinkedIn, Dev.to, and other social links
- A short bio / positioning statement
- Work experience (company, role, dates, what you built, results)
- Projects (name, description, GitHub link, tech stack)
- Skills (grouped by category)
- CV PDF filename

It then writes all the config files for you. No manual editing required.

### Option B — Manual setup

Edit these files directly:

```
_config.yml                  ← name, email, socials, blog RSS feed
_pages/about.md              ← bio content
assets/json/resume.json      ← full CV in JSONResume format
_data/repositories.yml       ← GitHub repos with descriptions
_data/socials.yml            ← social links
_projects/*.md               ← one file per case study
assets/img/ayan.png          ← replace with your photo
assets/pdf/your-cv.pdf       ← your CV PDF
```

---

## Local preview

Requires Docker.

```bash
docker compose up
```

Open [http://localhost:8080](http://localhost:8080).

---

## Deploy to GitHub Pages

```bash
# 1. Create a new repo named: yourusername.github.io
# 2. Push this folder to it
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin master

# 3. Go to repo Settings → Pages → Source → "Deploy from a branch" → gh-pages → Save
```

The GitHub Actions workflow builds the full Jekyll site (including custom plugins) and deploys automatically on every push. Live in ~2 minutes.

---

## What makes this different from the base al-folio template

| Feature | Base al-folio | This template |
|---------|--------------|---------------|
| Demo content removed | ❌ | ✅ |
| Dev.to RSS auto-pull | ❌ | ✅ |
| Clean blog list layout | ❌ | ✅ |
| Case studies page | ❌ | ✅ |
| Repo cards (no broken image deps) | ❌ | ✅ |
| Claude skill to fill everything in | ❌ | ✅ |

---

## Stack

- **Jekyll** — static site generator
- **al-folio** — base theme ([alshedivat/al-folio](https://github.com/alshedivat/al-folio))
- **GitHub Actions** — build and deploy pipeline
- **GitHub Pages** — hosting
- **JSONResume** — CV data format
- **Claude Code** — skill for automated setup

---

## Credits

This project is built on top of [al-folio](https://github.com/alshedivat/al-folio) by [@alshedivat](https://github.com/alshedivat) and contributors. All theme design, layout engine, and base components come from that project. This repo adds:

- Clean extraction with all demo content removed
- Dev.to RSS integration with content filtering
- Redesigned blog, case studies, and repositories pages
- A Claude Code skill for automated personalisation

If you use this template, consider starring [al-folio](https://github.com/alshedivat/al-folio) too.

---

## License

MIT — same as al-folio.
