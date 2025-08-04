# 🚀 Deploying a Professional Chirpy Jekyll Portfolio on GitHub Pages  
### *For [devsgh-cloudsec.github.io](https://devsgh-cloudsec.github.io)*  
Author: DevSingh ([devsgh-cloudsec](https://github.com/devsgh-cloudsec))  
Last Updated: July 2025

---

## 📚 Table of Contents
1. [Introduction](#introduction)
2. [Overview & Requirements](#overview--requirements)
3. [Step 1: Fork & Rename Chirpy Starter](#step-1-fork--rename-chirpy-starter)
4. [Step 2: Local Setup by Platform](#step-2-local-setup-by-platform)
    - [macOS Setup](#macos-setup)
    - [Ubuntu GNOME Setup](#ubuntu-gnome-setup)
    - [Windows Setup (WSL Recommended)](#windows-setup-wsl-recommended)
5. [Step 3: Clone Repo & Install Gems](#step-3-clone-repo--install-gems)
6. [Step 4: Run & Test Locally](#step-4-run--test-locally)
7. [Step 5: GitHub Actions Deployment](#step-5-github-actions-deployment)
8. [Step 6: Common Errors & Fixes](#step-6-common-errors--fixes)
9. [Step 7: Custom Domain (Optional)](#step-7-custom-domain-optional)
10. [Summary & Next Steps](#summary--next-steps)

---

## 🧠 Introduction

This guide walks you through deploying a **professional, production-grade Jekyll Chirpy site** on GitHub Pages, using your GitHub repo: `devsgh-cloudsec.github.io`.

It includes:
- Platform-specific setup for **macOS, Ubuntu, Windows**
- A foolproof GitHub Actions deployment process
- Troubleshooting of **real-world errors** like:
  - Raw front matter rendering (`--- layout: home ---`)
  - Broken workflows due to GitHub fork behavior

---

## ⚙️ Overview & Requirements

| Requirement        | Version/Tool          |
|--------------------|-----------------------|
| Ruby               | `3.2.x` (via rbenv)   |
| Bundler            | `2.5.6`               |
| Node.js (via NVM)  | `18.x`                |
| Jekyll             | `4.3+`                |
| GitHub Actions     | Required for deploy   |

---

## 🪝 Step 1: Fork & Rename Chirpy Starter

1. Fork the official starter:  
   👉 https://github.com/cotes2020/chirpy-starter

2. Rename the repo to:  
   ```
   devsgh-cloudsec.github.io
   ```

   ⚠️ This is required for GitHub Pages to auto-recognize it.

3. **Important:**  
   Forked repos **don’t auto-enable workflows**. You’ll fix that in [Step 5](#step-5-github-actions-deployment).

---

## 💻 Step 2: Local Setup by Platform

### macOS Setup

```bash
brew install ruby rbenv
brew install node
brew install git

# Add Ruby to path
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Then install:
```bash
rbenv install 3.2.2
rbenv global 3.2.2
gem install bundler
```

### Ubuntu GNOME Setup

```bash
sudo apt update && sudo apt install -y git curl   libssl-dev libreadline-dev zlib1g-dev autoconf bison   build-essential libyaml-dev libncurses5-dev libffi-dev libgdbm-dev

# Install rbenv and ruby
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init - bash)"' >> ~/.bashrc
source ~/.bashrc

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 3.2.2
rbenv global 3.2.2
gem install bundler
```

Install Node via NVM:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 18
nvm use 18
nvm alias default 18
```

### Windows Setup (WSL Recommended)

Install:
- [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/)
- Ubuntu via Microsoft Store

Then follow the **Ubuntu GNOME setup** steps above inside WSL.

---

## 📁 Step 3: Clone Repo & Install Gems

```bash
cd ~/Documents
git clone https://github.com/devsgh-cloudsec/devsgh-cloudsec.github.io.git
cd devsgh-cloudsec.github.io

bundle install
```

---

## 🔧 Step 4: Run & Test Locally

```bash
bundle exec jekyll serve
```

Visit: [http://localhost:4000](http://localhost:4000)

---

## 🚀 Step 5: GitHub Actions Deployment

### Fix for Forked Repo (Workflow Doesn’t Run):
Create this file:
```bash
mkdir -p .github/workflows

nano .github/workflows/pages-deploy.yml
```

Paste in the Chirpy GitHub Actions config (see GitHub docs or use template from previous assistant message).

Then push:

```bash
git add .github/workflows/pages-deploy.yml
git commit -m "Add GitHub Actions workflow for Chirpy deployment"
git push origin main
```

Now go to GitHub → **Settings → Pages** → Set **Build Source: GitHub Actions**

---

## ❌ Step 6: Common Errors & Fixes

### Raw Front Matter Shown (--- layout: home ---)
✅ **Fix:** GitHub was rendering source instead of compiled HTML. This is why GitHub Actions is critical.

### GitHub Actions Not Triggering
✅ Fix: Forked repos don’t auto-run workflows — you solved this by committing a new `.yml` file.

### Ruby Errors: Wrong Version
✅ Fix: Installed `rbenv` + `ruby 3.2.2` (instead of Ubuntu default 3.0.2)

---

## 🌐 Step 7: Custom Domain (Optional)

If using a custom domain like `devsingh.au` (VentraIP):

1. Create a file called `CNAME` in the root of the repo:
```
devsingh.au
```

2. In VentraIP DNS, create:

- `A` Records pointing to GitHub Pages IPs:  
  ```
  185.199.108.153  
  185.199.109.153  
  185.199.110.153  
  185.199.111.153
  ```

- Optional: CNAME for `www` → `devsingh.au`

---

## ✅ Summary & Next Steps

You now have:
- ✅ A professional GitHub Pages site at [devsgh-cloudsec.github.io](https://devsgh-cloudsec.github.io)
- ✅ Built with modern Jekyll + Ruby 3.2 + Bundler
- ✅ GitHub Actions CI for repeatable builds
- ✅ Ready to customize `_config.yml` with your profile, links, and content

**Next:** Customize your `_config.yml`, add your avatar and posts, and publish your cloud security content!

---

🛠️ Built and deployed by: [DevSingh](https://github.com/devsgh-cloudsec)  
🎯 Maintained via: GitHub Actions | Jekyll | Chirpy Theme
