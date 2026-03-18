# Workflow: Edit Public Repo → Save to Private Repo

## The Simple Approach

When you want to edit a public repository and push to your own private repository:

### 1. Make Changes Locally
Edit the files you need in your cloned repository.

### 2. Commit Changes
```bash
git add .
git commit -m "Your commit message"
```

### 3. Remove Original Remote
```bash
git remote remove origin
```

### 4. Add Your Private Repo as Origin
```bash
git remote add origin https://github.com/YOUR_USERNAME/your-private-repo.git
```

### 5. Push to Your Private Repo
```bash
git push -u origin main
```

## That's It!

Now all your changes are in your private repository, completely separate from the original public repo.

## If You Need to Pull Updates Later

Add the original as upstream first:
```bash
git remote add upstream https://github.com/OriginalOwner/original-repo.git
```

Then pull updates:
```bash
git fetch upstream
git merge upstream/main
git push origin main