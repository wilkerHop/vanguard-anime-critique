# AI Agent Tutorial: Publishing HTML to GitHub Pages with Commitlint

A comprehensive guide for AI agents to publish any HTML file to GitHub Pages with proper commit conventions.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Project Structure Setup](#project-structure-setup)
3. [Commitlint Configuration](#commitlint-configuration)
4. [GitHub Actions Workflow](#github-actions-workflow)
5. [Commit Message Conventions](#commit-message-conventions)
6. [Step-by-Step Publishing Process](#step-by-step-publishing-process)
7. [Automation Scripts](#automation-scripts)
8. [Troubleshooting](#troubleshooting)

## Prerequisites

Before starting, ensure you have:
- Git installed and configured
- Access to create GitHub repositories
- The HTML file to publish
- Basic understanding of terminal commands

## Project Structure Setup

### 1. Create Project Directory

```bash
# Create project directory
mkdir my-html-project
cd my-html-project

# Initialize git repository
git init
git branch -M main
```

### 2. Required Files Structure

```
my-html-project/
├── index.html              # Your main HTML file
├── README.md              # Project documentation
├── .github/
│   └── workflows/
│       └── deploy.yml     # GitHub Actions deployment
├── .commitlintrc.json     # Commitlint configuration
└── package.json           # Node.js dependencies (optional)
```

## Commitlint Configuration

### 1. Create `.commitlintrc.json`

```json
{
  "extends": ["@commitlint/config-conventional"],
  "rules": {
    "type-enum": [
      2,
      "always",
      [
        "feat",
        "fix",
        "docs",
        "style",
        "refactor",
        "perf",
        "test",
        "build",
        "ci",
        "chore",
        "revert"
      ]
    ],
    "type-case": [2, "always", "lower-case"],
    "type-empty": [2, "never"],
    "subject-empty": [2, "never"],
    "subject-full-stop": [2, "never", "."],
    "header-max-length": [2, "always", 100]
  }
}
```

### 2. Commit Types Reference

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | `feat: add contact form to homepage` |
| `fix` | Bug fix | `fix: correct navigation menu alignment` |
| `docs` | Documentation | `docs: update README with deployment steps` |
| `style` | Code style changes | `style: apply neo-brutalist design` |
| `refactor` | Code refactoring | `refactor: reorganize CSS structure` |
| `perf` | Performance improvements | `perf: optimize image loading` |
| `test` | Adding tests | `test: add validation for form inputs` |
| `build` | Build system changes | `build: update webpack configuration` |
| `ci` | CI/CD changes | `ci: add GitHub Actions workflow` |
| `chore` | Maintenance tasks | `chore: update dependencies` |
| `revert` | Revert previous commit | `revert: undo navigation changes` |

## GitHub Actions Workflow

### Create `.github/workflows/deploy.yml`

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Pages
        uses: actions/configure-pages@v4
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## Commit Message Conventions

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Rules

1. **Type**: Required, lowercase, from allowed list
2. **Scope**: Optional, describes section affected
3. **Subject**: Required, concise description (no period)
4. **Body**: Optional, detailed explanation
5. **Footer**: Optional, breaking changes or issue references

### Examples

#### ✅ Good Commits

```bash
# Simple feature
git commit -m "feat: add navigation menu"

# With scope
git commit -m "feat(header): add responsive navigation menu"

# With body
git commit -m "feat: add contact form

- Add email validation
- Add phone number field
- Add submit button with styling"

# Breaking change
git commit -m "feat!: redesign homepage layout

BREAKING CHANGE: Old CSS classes removed, update custom styles"

# Bug fix
git commit -m "fix: correct mobile menu toggle"

# Documentation
git commit -m "docs: add deployment instructions to README"

# Style changes
git commit -m "style: apply neo-brutalist design system"
```

#### ❌ Bad Commits

```bash
# No type
git commit -m "added new feature"

# Wrong type case
git commit -m "Feat: add menu"

# Period at end
git commit -m "feat: add menu."

# Too vague
git commit -m "fix: stuff"

# Too long subject
git commit -m "feat: add a really long description that exceeds the maximum allowed length for commit messages"
```

## Step-by-Step Publishing Process

### Step 1: Prepare Your HTML File

```bash
# Place your HTML file in the project directory
cp /path/to/your/file.html index.html
```

### Step 2: Create README.md

```markdown
# Project Name

[![Deploy to GitHub Pages](https://github.com/USERNAME/REPO/actions/workflows/deploy.yml/badge.svg)](https://github.com/USERNAME/REPO/actions/workflows/deploy.yml)

## Description

Brief description of your HTML project.

## Live Demo

Visit: https://USERNAME.github.io/REPO/

## Local Development

```bash
# Open in browser
open index.html
```

## Deployment

Automatically deployed to GitHub Pages via GitHub Actions.
```

### Step 3: Initialize Git and Make Initial Commit

```bash
# Add all files
git add .

# Commit with proper format
git commit -m "chore: initial project setup

- Add index.html
- Add README.md
- Configure GitHub Actions workflow
- Add commitlint configuration"
```

### Step 4: Create GitHub Repository

```bash
# Using GitHub CLI (recommended)
gh repo create my-html-project --public --source=. --remote=origin --push

# Or manually:
# 1. Go to https://github.com/new
# 2. Create repository (don't initialize with README)
# 3. Run:
git remote add origin https://github.com/USERNAME/REPO.git
git push -u origin main
```

### Step 5: Enable GitHub Pages

```bash
# Using GitHub CLI
gh api repos/USERNAME/REPO/pages -X POST -f source[branch]=main -f source[path]=/

# Or manually:
# 1. Go to repository Settings → Pages
# 2. Set Source to "GitHub Actions"
```

### Step 6: Verify Deployment

```bash
# Check workflow status
gh run list --limit 5

# View workflow logs
gh run view

# Open deployed site
open https://USERNAME.github.io/REPO/
```

## Automation Scripts

### Script 1: `publish.sh` - Complete Publishing Script

```bash
#!/bin/bash
set -e

# Configuration
REPO_NAME="${1:-my-html-project}"
HTML_FILE="${2:-index.html}"
COMMIT_TYPE="${3:-feat}"
COMMIT_MSG="${4:-add HTML website}"

echo "🚀 Publishing HTML to GitHub Pages..."
echo "Repository: $REPO_NAME"
echo "HTML File: $HTML_FILE"

# Create project structure
mkdir -p "$REPO_NAME/.github/workflows"
cd "$REPO_NAME"

# Copy HTML file
cp "../$HTML_FILE" index.html

# Create GitHub Actions workflow
cat > .github/workflows/deploy.yml << 'EOF'
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v4
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
EOF

# Create commitlint config
cat > .commitlintrc.json << 'EOF'
{
  "extends": ["@commitlint/config-conventional"]
}
EOF

# Create README
cat > README.md << EOF
# $REPO_NAME

[![Deploy to GitHub Pages](https://github.com/\$GITHUB_USERNAME/$REPO_NAME/actions/workflows/deploy.yml/badge.svg)](https://github.com/\$GITHUB_USERNAME/$REPO_NAME/actions/workflows/deploy.yml)

## Live Demo

Visit: https://\$GITHUB_USERNAME.github.io/$REPO_NAME/

## Deployment

Automatically deployed to GitHub Pages via GitHub Actions.
EOF

# Initialize git
git init
git branch -M main

# Add files
git add .

# Commit with proper format
git commit -m "$COMMIT_TYPE: $COMMIT_MSG

- Add index.html
- Configure GitHub Actions deployment
- Add README with status badge"

# Create GitHub repository and push
gh repo create "$REPO_NAME" --public --source=. --remote=origin --push

# Enable GitHub Pages
sleep 2
gh api "repos/\$(gh repo view --json owner -q .owner.login)/$REPO_NAME/pages" \
  -X POST -f source[branch]=main -f source[path]=/ || echo "Pages may already be enabled"

echo "✅ Published successfully!"
echo "🌐 Your site will be available at: https://\$(gh repo view --json owner -q .owner.login).github.io/$REPO_NAME/"
```

### Script 2: `commit-helper.sh` - Commitlint Helper

```bash
#!/bin/bash

# Commit helper with validation
commit_with_lint() {
    local type=$1
    local scope=$2
    local subject=$3
    local body=$4
    
    # Validate type
    valid_types=("feat" "fix" "docs" "style" "refactor" "perf" "test" "build" "ci" "chore" "revert")
    if [[ ! " ${valid_types[@]} " =~ " ${type} " ]]; then
        echo "❌ Invalid type: $type"
        echo "Valid types: ${valid_types[*]}"
        return 1
    fi
    
    # Build commit message
    if [ -n "$scope" ]; then
        msg="$type($scope): $subject"
    else
        msg="$type: $subject"
    fi
    
    if [ -n "$body" ]; then
        msg="$msg

$body"
    fi
    
    # Commit
    git commit -m "$msg"
    echo "✅ Committed: $msg"
}

# Usage examples:
# commit_with_lint "feat" "" "add navigation menu"
# commit_with_lint "fix" "header" "correct mobile alignment"
# commit_with_lint "docs" "" "update README" "Add deployment instructions"
```

### Script 3: `update-and-deploy.sh` - Update Existing Project

```bash
#!/bin/bash
set -e

# Update HTML and deploy
HTML_FILE="${1:-index.html}"
COMMIT_MSG="${2:-update website}"

echo "📝 Updating HTML file..."

# Copy new HTML
cp "$HTML_FILE" index.html

# Stage changes
git add index.html

# Check if there are changes
if git diff --staged --quiet; then
    echo "ℹ️  No changes detected"
    exit 0
fi

# Commit with proper format
git commit -m "feat: $COMMIT_MSG"

# Push to trigger deployment
git push origin main

echo "✅ Updated and deployed!"
echo "⏳ Check deployment status: gh run watch"
```

## Troubleshooting

### Issue: Commit Rejected by Commitlint

**Solution**: Ensure commit message follows convention:
```bash
# Check commit message format
git log -1 --pretty=%B

# Amend last commit if needed
git commit --amend -m "feat: correct commit message"
```

### Issue: GitHub Pages Not Deploying

**Solution**:
```bash
# Check workflow status
gh run list

# View workflow logs
gh run view --log

# Verify Pages is enabled
gh api repos/OWNER/REPO/pages
```

### Issue: Badge Not Showing

**Solution**: Update badge URL in README.md:
```markdown
[![Deploy](https://github.com/YOUR_USERNAME/YOUR_REPO/actions/workflows/deploy.yml/badge.svg)](https://github.com/YOUR_USERNAME/YOUR_REPO/actions/workflows/deploy.yml)
```

### Issue: Permission Denied on Push

**Solution**:
```bash
# Configure git credentials
gh auth login

# Or use SSH
git remote set-url origin git@github.com:USERNAME/REPO.git
```

## Best Practices for AI Agents

### 1. Always Validate Before Committing

```bash
# Check what will be committed
git status
git diff --staged

# Validate commit message format before committing
echo "feat: add new feature" | npx commitlint
```

### 2. Use Atomic Commits

Each commit should represent one logical change:
```bash
# Good: Separate commits for different changes
git commit -m "feat: add navigation menu"
git commit -m "style: apply responsive design to navigation"

# Bad: Multiple unrelated changes in one commit
git commit -m "feat: add menu and fix bugs and update docs"
```

### 3. Write Descriptive Commit Messages

```bash
# Good: Clear and specific
git commit -m "fix: correct mobile menu toggle on iOS Safari"

# Bad: Vague
git commit -m "fix: menu issue"
```

### 4. Test Locally Before Pushing

```bash
# Open HTML in browser
open index.html

# Or use local server
python3 -m http.server 8000
```

### 5. Monitor Deployment

```bash
# Watch deployment in real-time
gh run watch

# Check deployment status
gh run list --limit 1
```

## Complete Example Workflow

Here's a complete example of publishing an HTML file:

```bash
# 1. Create project directory
mkdir awesome-website
cd awesome-website

# 2. Copy your HTML file
cp ~/my-website.html index.html

# 3. Create workflow directory
mkdir -p .github/workflows

# 4. Create deployment workflow
cat > .github/workflows/deploy.yml << 'EOF'
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      - uses: actions/deploy-pages@v4
EOF

# 5. Create README
cat > README.md << 'EOF'
# Awesome Website

[![Deploy](https://github.com/USERNAME/awesome-website/actions/workflows/deploy.yml/badge.svg)](https://github.com/USERNAME/awesome-website/actions/workflows/deploy.yml)

Live at: https://USERNAME.github.io/awesome-website/
EOF

# 6. Create commitlint config
cat > .commitlintrc.json << 'EOF'
{
  "extends": ["@commitlint/config-conventional"]
}
EOF

# 7. Initialize git
git init
git branch -M main

# 8. Make initial commit
git add .
git commit -m "chore: initial project setup

- Add index.html
- Configure GitHub Actions deployment
- Add README with deployment badge
- Add commitlint configuration"

# 9. Create GitHub repository and push
gh repo create awesome-website --public --source=. --remote=origin --push

# 10. Enable GitHub Pages
gh api repos/$(gh repo view --json owner -q .owner.login)/awesome-website/pages \
  -X POST -f source[branch]=main -f source[path]=/

# 11. Monitor deployment
gh run watch

# 12. Open deployed site
open https://$(gh repo view --json owner -q .owner.login).github.io/awesome-website/

echo "✅ Website published successfully!"
```

## Summary

This tutorial provides AI agents with a complete workflow for publishing HTML files to GitHub Pages with proper commit conventions. Key takeaways:

1. **Always use commitlint-compliant commit messages**
2. **Set up GitHub Actions for automatic deployment**
3. **Include status badges in documentation**
4. **Test locally before pushing**
5. **Monitor deployment status**
6. **Use automation scripts for efficiency**

For questions or issues, refer to:
- [Commitlint Documentation](https://commitlint.js.org/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
