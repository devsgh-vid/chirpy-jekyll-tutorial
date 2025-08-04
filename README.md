# 🚀 Deploying a Professional Chirpy Jekyll Portfolio on GitHub Pages  
### Ubuntu 22.04 Desktop (Fresh Install – Beginner Friendly)

Author: DevSingh (https://github.com/devsgh-cloudsec)  
Last Updated: July 2025

---

## 📚 Table of Contents
1. [System Setup (Ubuntu 22.04)](#1-system-setup-ubuntu-2204)
2. [Install Git & Global Config](#2-install-git--global-config)
3. [Install Ruby (via rbenv)](#3-install-ruby-via-rbenv)
4. [Install Node.js (via NVM)](#4-install-nodejs-via-nvm)
5. [Install Jekyll & Bundler](#5-install-jekyll--bundler)
6. [Fork & Clone Chirpy Repository](#6-fork--clone-chirpy-repository)
7. [Install Dependencies](#7-install-dependencies)
8. [Run Your Site Locally](#8-run-your-site-locally)
9. [Deploy via GitHub Actions](#9-deploy-via-github-actions)
10. [Final Checks & Troubleshooting](#10-final-checks--troubleshooting)

---

## 1. ✅ System Setup (Ubuntu 22.04)

```bash
sudo apt update && sudo apt upgrade -y

# Install essential dev tools & libraries
sudo apt install -y git curl wget build-essential zlib1g-dev libssl-dev libreadline-dev libyaml-dev libffi-dev libgdbm-dev libncurses5-dev libgmp-dev libgmp-dev autoconf bison libgdbm-compat-dev libgmp-dev libsqlite3-dev

# Fix locale issues
sudo apt install -y locales
sudo locale-gen en_US.UTF-8
```

---

## 2. ✅ Install Git & Global Config

```bash
sudo apt install -y git

git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

---

## 3. ✅ Install Ruby (via rbenv – Production Grade)

```bash
# Install rbenv and ruby-build
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init - bash)"' >> ~/.bashrc
source ~/.bashrc

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

# Install Ruby 3.2.2
rbenv install 3.2.2
rbenv global 3.2.2

# Confirm
ruby -v
```

---

## 4. ✅ Install Node.js (via NVM)

```bash
# Clean up old/broken installs first
sudo apt remove -y nodejs npm
sudo apt autoremove -y
sudo rm -f /usr/bin/node /usr/bin/npm

# Install NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc

# Install Node 18 LTS
nvm install 18
nvm use 18
nvm alias default 18

# Confirm
node -v
npm -v
```

---

## 5. ✅ Install Jekyll & Bundler

```bash
gem install bundler -v 2.5.6
gem install jekyll
```

---

## 6. ✅ Fork & Clone Chirpy Repository

1. Go to: https://github.com/cotes2020/chirpy-starter  
2. Click “Fork” → your GitHub account  
3. Rename it to: `devsgh-cloudsec.github.io`

Then clone locally:

```bash
cd ~/Documents
git clone https://github.com/devsgh-cloudsec/devsgh-cloudsec.github.io.git
cd devsgh-cloudsec.github.io
```

---

## 7. ✅ Install Dependencies (Bundle Install)

```bash
# Set correct Ruby platform for GitHub Actions
bundle _2.5.6_ lock --add-platform x86_64-linux

# Clean install
rm -rf .bundle Gemfile.lock
bundle _2.5.6_ install
```

If you see errors like:
- `json`, `ffi`, or `racc` native extensions failed:
```bash
gem pristine --all
bundle _2.5.6_ install
```

---

## 8. ✅ Run Your Site Locally

```bash
bundle _2.5.6_ exec jekyll serve --livereload
```

Visit: http://localhost:4000

You should see the Chirpy theme running locally.

---

## 9. ✅ Deploy via GitHub Actions (Not Branch!)

### Setup Deployment Workflow

```bash
mkdir -p .github/workflows

nano .github/workflows/pages-deploy.yml
```

Paste this (official Chirpy deployment):

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'
          bundler-cache: true

      - run: bundle install
      - run: bundle exec jekyll build
        env:
          JEKYLL_ENV: production

      - uses: actions/upload-pages-artifact@v3
        with:
          path: _site/

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

Commit and push:

```bash
git add .
git commit -m "Enable GitHub Actions deployment for Chirpy"
git push origin main
```

---

### In GitHub UI:
1. Go to `Settings → Pages`
2. Under "Build & Deployment", choose:  
   ✅ **Source: GitHub Actions**
3. Save and wait for the build

---

## 10. ✅ Final Checks & Troubleshooting

- [x] Site builds locally? `bundle exec jekyll serve`
- [x] GitHub Actions runs successfully? (green checkmark)
- [x] `https://devsgh-cloudsec.github.io` shows Chirpy homepage?

---

## 🧠 Common Fixes

| Issue | Fix |
|-------|-----|
| `--- layout: home ---` raw | GitHub Pages not building → Use Actions |
| `nokogiri` fails | `apt install libxslt-dev libxml2-dev` |
| `ffi`, `racc`, etc. fail | Run `gem pristine --all` |

---

🎉 You’ve now deployed a **production-grade Jekyll blog** on GitHub Pages from a fresh Ubuntu install — with full control, CI/CD, and Chirpy's power.

Maintain your `_config.yml`, posts, and GitHub Actions to grow your site over time.

---

🛠 Maintained by: [DevSingh](https://github.com/devsgh-cloudsec)
