# Quick Deployment Checklist

## Pre-Deployment Setup

### 1. Create GitHub Repository
- [ ] Create new repository: `pr0jzer0.github.io`
- [ ] Make it **public** (required for GitHub Pages)
- [ ] Do NOT initialize with README (we'll push our own)

### 2. Local Setup
```bash
# Create project directory
mkdir pr0jzer0.github.io && cd pr0jzer0.github.io

# Initialize git
git init

# Create directory structure
mkdir -p .github/workflows content/{root,lab/posts,sieve,about} static/{images,files} archetypes

# Add Terminal theme
git submodule add https://github.com/panr/hugo-theme-terminal.git themes/terminal
```

### 3. Copy Files
Copy these files from the project:
- [ ] `hugo.yaml` → root directory
- [ ] `.github/workflows/hugo.yml` → `.github/workflows/`
- [ ] `content/_index.md` → `content/`
- [ ] `.gitignore` → root directory
- [ ] `archetypes/default.md` → `archetypes/`
- [ ] Create content files for `/root`, `/lab`, `/sieve`, `/about`

### 4. Test Locally
```bash
# Install Hugo (if not installed)
# macOS: brew install hugo
# Linux: sudo apt install hugo
# Windows: choco install hugo-extended

# Start local server
hugo server -D

# Visit http://localhost:1313
# Verify all pages load correctly
```

### 5. Push to GitHub
```bash
# Add all files
git add .

# Commit
git commit -m "Initial commit: nulx0x00 portfolio with Terminal theme"

# Add remote
git remote add origin https://github.com/pr0jzer0/pr0jzer0.github.io.git

# Push
git push -u origin main
```

### 6. Configure GitHub Pages
1. Go to repository **Settings**
2. Navigate to **Pages** (left sidebar)
3. Under "Build and deployment":
   - Source: Select **GitHub Actions**
4. Wait 2-3 minutes for first deployment

### 7. Verify Deployment
- [ ] Check **Actions** tab for workflow status (should be green ✓)
- [ ] Visit `https://pr0jzer0.github.io`
- [ ] Test all menu links: /root, /lab, /sieve, /whoami

## Post-Deployment

### Regular Content Updates
```bash
# Create new blog post
hugo new lab/posts/my-new-post.md

# Edit the file, set draft: false when ready

# Push to deploy
git add .
git commit -m "Add new post: [title]"
git push
```

### Update Site Configuration
Edit `hugo.yaml` for:
- Menu items
- Social links
- Theme colors
- Site description

### Add Images/Files
Place in `static/` directory:
```
static/
├── images/
│   ├── profile.png
│   └── sieve-diagram.png
└── files/
    └── tools/
        └── nulx-sieve-v1.0.zip
```

Reference in markdown:
```markdown
![Diagram](/images/sieve-diagram.png)
[Download Tool](/files/tools/nulx-sieve-v1.0.zip)
```

## Customization Quick Wins

### 1. Update Email/Contact
Edit `hugo.yaml`:
```yaml
social:
  - name: "email"
    url: "mailto:your-actual-email@protonmail.com"
```

### 2. Change Theme Color
Edit `hugo.yaml`:
```yaml
params:
  themeColor: "green"  # Options: orange, red, blue, green, pink
```

### 3. Add Custom Fonts
Create `assets/css/custom.css`:
```css
@import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;700&display=swap');

code, pre {
  font-family: 'Fira Code', monospace;
}
```

## Troubleshooting

**Build fails?**
- Check `.github/workflows/hugo.yml` syntax
- Verify theme submodule: `git submodule update --init --recursive`

**Pages not showing?**
- Ensure `draft: false` in front matter
- Check file paths match menu URLs

**Theme not loading?**
- Reinit submodules: `git submodule update --init --recursive`
- Verify `theme: "terminal"` in `hugo.yaml`

## Maintenance Commands

```bash
# Update Hugo theme
cd themes/terminal
git pull origin master
cd ../..
git add themes/terminal
git commit -m "Update Terminal theme"
git push

# Clean build
hugo --gc --minify

# Test production build locally
hugo server --environment production
```

---

**Your Site**: https://pr0jzer0.github.io  
**Deployment**: Automatic on push to `main`  
**Build Time**: ~2-3 minutes
