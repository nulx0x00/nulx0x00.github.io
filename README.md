# nulx0x00 Portfolio - Hugo Setup Guide

## Project Structure

```
pr0jzer0.github.io/
├── .github/
│   └── workflows/
│       └── hugo.yml                 # GitHub Actions deployment workflow
├── archetypes/
│   └── default.md                   # Template for new content
├── content/
│   ├── _index.md                    # Homepage content
│   ├── root/
│   │   └── _index.md                # /root section (bio)
│   ├── lab/
│   │   ├── _index.md                # /lab landing page
│   │   └── posts/                   # Individual research posts
│   │       ├── osi-model-layer1.md
│   │       └── python-recon-tools.md
│   ├── sieve/
│   │   └── _index.md                # NulX Sieve project page
│   └── about/
│       └── _index.md                # /whoami page
├── static/
│   ├── images/                      # Images and assets
│   └── files/                       # Downloadable files (tools, PDFs)
├── themes/
│   └── terminal/                    # Terminal theme (git submodule)
├── hugo.yaml                        # Main Hugo configuration
└── README.md
```

## Setup Instructions

### 1. Install Hugo

**macOS:**
```bash
brew install hugo
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt install hugo
```

**Windows (with Chocolatey):**
```bash
choco install hugo-extended
```

Verify installation:
```bash
hugo version
```

### 2. Create Your Local Repository

```bash
# Create project directory
mkdir pr0jzer0.github.io
cd pr0jzer0.github.io

# Initialize git repository
git init

# Create necessary directories
mkdir -p .github/workflows
mkdir -p content/{root,lab/posts,sieve,about}
mkdir -p static/{images,files}
mkdir -p archetypes
```

### 3. Add the Terminal Theme

```bash
# Add Terminal theme as a git submodule
git submodule add https://github.com/panr/hugo-theme-terminal.git themes/terminal
git submodule update --init --recursive
```

### 4. Copy Configuration Files

Place the provided files in the following locations:

- `hugo.yaml` → Root directory
- `.github/workflows/hugo.yml` → `.github/workflows/`
- `content/_index.md` → `content/` (homepage)

### 5. Create Additional Content Files

**Create `/root/_index.md`:**
```bash
cat > content/root/_index.md << 'EOF'
---
title: "/root - Bio"
date: 2025-01-31
draft: false
---

## Access Granted: /root

[Your detailed bio focusing on stripping security myths]

### Background
- Logic-based approach to security research
- Focus on OSI model fundamentals
- Builder of practical offensive tools

### Philosophy
"Security controls should be explainable at the packet level. If you can't trace an alert from Layer 7 down to Layer 1, you don't understand it."

EOF
```

**Create `/lab/_index.md`:**
```bash
cat > content/lab/_index.md << 'EOF'
---
title: "/lab - Research & Write-ups"
date: 2025-01-31
draft: false
---

## nulx0x00:/lab$

Deep-dive technical research on network protocols, exploit development, and offensive security tooling.

### Research Areas
- OSI Model Layer Analysis (1-7)
- Python Offensive Security Tools
- Network Protocol Manipulation
- Automated Reconnaissance Frameworks

---

## Recent Publications

EOF
```

**Create `/sieve/_index.md`:**
```bash
cat > content/sieve/_index.md << 'EOF'
---
title: "NulX Sieve - PII Discovery Tool"
date: 2025-01-31
draft: false
---

# NulX Sieve

## Automated PII Discovery & Data Exposure Analysis

**Status**: Active Development  
**Language**: Python 3.10+  
**License**: MIT

### Overview

NulX Sieve is a high-performance tool for discovering personally identifiable information (PII) across file systems, databases, and APIs. Unlike bloated enterprise solutions, Sieve focuses on speed, accuracy, and actionable results.

### Key Features

- **Multi-Source Scanning**: File systems, databases (SQL/NoSQL), REST APIs
- **Pattern Detection**: Regex-based + ML-assisted classification
- **Performance**: Async I/O for high-speed scanning
- **Output Formats**: JSON, CSV, HTML reports
- **Zero Dependencies**: Minimal external libraries for portability

### Installation

```bash
git clone https://github.com/pr0jzer0/nulx-sieve
cd nulx-sieve
pip install -r requirements.txt
```

### Usage

```bash
# Scan a directory
python sieve.py scan --path /var/www --output report.json

# Scan with specific patterns
python sieve.py scan --path /data --patterns email,ssn,credit-card

# Generate HTML report
python sieve.py report --input report.json --format html
```

### Architecture

[Technical architecture details]

### Roadmap

- [ ] Database connector modules
- [ ] Cloud storage integration (S3, Azure Blob)
- [ ] Real-time monitoring mode
- [ ] Custom pattern builder GUI

---

**GitHub**: [pr0jzer0/nulx-sieve](https://github.com/pr0jzer0/nulx-sieve)

EOF
```

### 6. Create Sample Lab Post

```bash
cat > content/lab/posts/osi-layer1-analysis.md << 'EOF'
---
title: "OSI Layer 1: Physical Layer Security Analysis"
date: 2025-01-31
draft: false
tags: ["OSI Model", "Network Security", "Physical Layer"]
categories: ["Research"]
---

## Introduction

The physical layer is where all security ultimately begins—and where it's most often ignored.

### Key Concepts

1. **Signal Analysis**: Understanding electromagnetic emanations
2. **Cable Security**: Why fiber optics matter for sensitive data
3. **Physical Access**: The ultimate vulnerability

[Your detailed research content here]

### Practical Implications

- Air-gapped systems aren't air-gapped if you can measure EM radiation
- Physical layer attacks bypass almost all higher-layer controls
- Hardware security modules (HSMs) exist for a reason

### Tools & Techniques

```python
# Example: Basic signal analysis
import numpy as np
from scipy import signal

# Analyze signal characteristics
def analyze_physical_signal(data):
    frequencies = np.fft.fft(data)
    return signal.find_peaks(frequencies)
```

---

**Next in Series**: Layer 2 - Data Link Security
EOF
```

### 7. Initialize GitHub Repository

```bash
# Add all files
git add .

# Commit
git commit -m "Initial commit: Hugo site structure with Terminal theme"

# Add remote (create pr0jzer0.github.io repo on GitHub first)
git remote add origin https://github.com/pr0jzer0/pr0jzer0.github.io.git

# Push to GitHub
git push -u origin main
```

### 8. Configure GitHub Pages

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages**
3. Under "Build and deployment":
   - Source: **GitHub Actions**
4. The workflow will automatically trigger on the next push

### 9. Local Development

Test your site locally before pushing:

```bash
# Start Hugo development server
hugo server -D

# Visit http://localhost:1313
```

Build the site locally:

```bash
# Generate static files
hugo

# Output will be in ./public/
```

## Customization Tips

### Update Social Links

Edit `hugo.yaml`:
```yaml
social:
  - name: "github"
    url: "https://github.com/pr0jzer0"
  - name: "email"
    url: "mailto:your-email@protonmail.com"
```

### Change Theme Colors

Available colors in Terminal theme: `orange`, `red`, `blue`, `green`, `pink`

Edit in `hugo.yaml`:
```yaml
params:
  themeColor: "green"
```

### Add Custom CSS

Create `assets/css/custom.css`:
```css
/* Custom terminal-style overrides */
.container {
  max-width: 900px;
}

code {
  font-family: 'Fira Code', 'Courier New', monospace;
}
```

## Deployment Workflow

Every time you push to `main`:

1. GitHub Actions triggers the workflow
2. Hugo builds the site
3. Site deploys to `https://pr0jzer0.github.io/`
4. Typically completes in 2-3 minutes

Check deployment status: **Actions** tab in your GitHub repository

## Troubleshooting

**Issue**: Theme not loading
```bash
# Reinitialize submodules
git submodule update --init --recursive
```

**Issue**: Build fails on GitHub
- Check `.github/workflows/hugo.yml` is in the correct location
- Verify GitHub Pages is set to "GitHub Actions" source

**Issue**: Content not appearing
- Ensure `draft: false` in front matter
- Check file is in correct `content/` subdirectory

## Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Terminal Theme Docs](https://github.com/panr/hugo-theme-terminal)
- [GitHub Pages Docs](https://docs.github.com/en/pages)

---

**Site**: https://pr0jzer0.github.io  
**Author**: nulx0x00  
**Updated**: 2025-01-31
