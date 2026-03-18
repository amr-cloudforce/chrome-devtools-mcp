# Workflow for Editing Public Repositories

This document describes the workflow for making changes to a public repository and pushing to your own private repository.

## Scenario

You want to:
1. Make changes to a public GitHub repository (e.g., a fork of chrome-devtools-mcp)
2. Push those changes to your own private repository
3. Keep your changes separate from the original public repo

## Step-by-Step Workflow

### 1. Clone or Work with Existing Clone

If you already have the repo cloned:
```bash
cd /path/to/repository
```

Or clone the original public repo:
```bash
git clone https://github.com/OriginalOwner/original-repo.git
cd original-repo
```

### 2. Make Your Changes

Edit the files you need to change in your local repository.

### 3. Build and Test Locally

Make sure your changes work before pushing:
```bash
npm install
npm run build
npm test  # if tests exist
```

### 4. Check Your Changes

See what files have been modified:
```bash
git status
```

View specific changes:
```bash
git diff src/some-file.ts
```

### 5. Stage and Commit Changes

Add all changes:
```bash
git add .
```

Or add specific files:
```bash
git add src/browser.ts src/bin/chrome-devtools-mcp-cli-options.ts
```

Commit with a descriptive message:
```bash
git commit -m "Enable Chrome extension support"
```

### 6. Remove Existing Remote (if needed)

Check current remotes:
```bash
git remote -v
```

If `origin` already exists and points to the original repo, remove it:
```bash
git remote remove origin
```

### 7. Add Your Private Repo as Remote

Add your private repository as the new `origin`:
```bash
git remote add origin https://github.com/YOUR_USERNAME/your-private-repo.git
```

Verify the remote was added correctly:
```bash
git remote -v
```

You should see:
```
origin  https://github.com/YOUR_USERNAME/your-private-repo.git (fetch)
origin  https://github.com/YOUR_USERNAME/your-private-repo.git (push)
```

### 8. Push to Your Private Repo

Push your changes to your private repository:
```bash
git push -u origin main
```

The `-u` flag sets the upstream branch, so future pushes can be done with just `git push`.

### 9. Verify on GitHub

1. Go to your private repository on GitHub: `https://github.com/YOUR_USERNAME/your-private-repo`
2. Verify your changes are there
3. Check the commit history

## Alternative: Keep Original Remote and Add Your Own

If you want to keep a reference to the original repo for future updates:

### Add Original as `upstream`

```bash
git remote add upstream https://github.com/OriginalOwner/original-repo.git
```

### Add Your Private Repo as `origin`

```bash
git remote add origin https://github.com/YOUR_USERNAME/your-private-repo.git
```

### Verify Both Remotes

```bash
git remote -v
```

You should see:
```
origin    https://github.com/YOUR_USERNAME/your-private-repo.git (fetch)
origin    https://github.com/YOUR_USERNAME/your-private-repo.git (push)
upstream  https://github.com/OriginalOwner/original-repo.git (fetch)
upstream  https://github.com/OriginalOwner/original-repo.git (push)
```

### Push to Your Repo

```bash
git push -u origin main
```

### Pull Latest Changes from Original (Optional)

To get updates from the original repo in the future:
```bash
git fetch upstream
git merge upstream/main
```

## Common Issues and Solutions

### Error: "remote origin already exists"

**Problem:** You already have a remote named `origin`.

**Solution:** Remove it first:
```bash
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/your-private-repo.git
```

### Error: "failed to push some refs"

**Problem:** Your private repo has commits that don't exist on your local machine.

**Solution:** Pull first, then push (be careful - this may overwrite remote changes):
```bash
git pull origin main --allow-unrelated-histories
git push origin main
```

Or force push (use with caution - this overwrites remote):
```bash
git push -f origin main
```

### Want to Keep Original Repo Up to Date

If you periodically want to merge changes from the original public repo:

```bash
# Fetch latest from upstream
git fetch upstream

# Merge upstream changes into your main branch
git checkout main
git merge upstream/main

# Push to your private repo
git push origin main
```

## Best Practices

1. **Always test locally before pushing** - Build and run tests
2. **Use descriptive commit messages** - Explain what you changed and why
3. **Keep documentation updated** - Update READMEs or create new docs like this one
4. **Consider branching** - For larger projects, use feature branches:
   ```bash
   git checkout -b feature/my-changes
   # make changes
   git push origin feature/my-changes
   ```
5. **Backup important work** - Push to your private repo regularly

## Example: Chrome DevTools MCP

Here's a complete example for the changes we made:

```bash
# Clone the original repo
git clone https://github.com/ChromeDevTools/chrome-devtools-mcp.git
cd chrome-devtools-mcp

# Make changes to src/browser.ts and src/bin/chrome-devtools-mcp-cli-options.ts

# Build and test
npm install
npm run build

# Commit changes
git add .
git commit -m "Enable Chrome extension support"

# Remove original remote
git remote remove origin

# Add your private repo
git remote add origin https://github.com/amr-cloudforce/chrome-devtools-mcp

# Push to your private repo
git push -u origin main
```

## Date

March 18, 2026