# GitHub Repo Creator PWA

A fast, minimal PWA for creating GitHub repos from your phone — with Apple Shortcuts integration.

## Installation

### 1. Host the HTML file

Upload `github-repo-creator.html` to any static host:

- **GitHub Pages**: Push to a repo, enable Pages
- **Netlify/Vercel**: Drag & drop the file
- **Your own server**: Just serve the HTML file
- **Cloudflare Pages**: Free, fast, easy

Example URL after hosting: `https://yoursite.com/github-repo-creator.html`

### 2. Add to Home Screen

1. Open the URL in Safari (iPhone) or Chrome (Android)
2. **iPhone**: Tap Share → "Add to Home Screen"
3. **Android**: Tap ⋮ menu → "Add to Home Screen"

### 3. First Launch Setup

1. Open the app from your home screen
2. Tap the gear icon (⚙️)
3. Paste your GitHub Personal Access Token
4. Tap "Save Token"

#### Creating a GitHub Token

1. Go to GitHub → Settings → Developer settings
2. Personal access tokens → Tokens (classic)
3. Generate new token (classic)
4. Check the `repo` scope
5. Generate and copy the token

---

## Apple Shortcuts Integration

### URL Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `name` | Repo name (required for auto) | `my-project` |
| `desc` | Description (optional) | `A%20cool%20project` |
| `private` | `true` or `false` | `true` (default) |
| `auto` | Auto-create on load | `true` |

### Example URLs

**Pre-fill form only:**
```
https://yoursite.com/github-repo-creator.html?name=my-project&desc=My%20description
```

**Auto-create (hands-free):**
```
https://yoursite.com/github-repo-creator.html?name=my-project&desc=My%20description&auto=true
```

---

## Apple Shortcut Setup

### Option A: Simple (Ask for Name)

1. Open **Shortcuts** app → tap **+**
2. Add action: **Ask for Input**
   - Prompt: `Project name`
   - Input Type: Text
3. Add action: **URL**
   - Value: `https://yoursite.com/github-repo-creator.html?name=[Provided Input]&auto=true`
4. Add action: **Open URLs**
5. Name it "New Repo" and pick an icon

### Option B: With Description

1. Open **Shortcuts** app → tap **+**
2. Add action: **Ask for Input**
   - Prompt: `Project name`
   - Input Type: Text
   - Save as variable: `RepoName`
3. Add action: **Ask for Input**
   - Prompt: `Description (optional)`
   - Input Type: Text
   - Save as variable: `RepoDesc`
4. Add action: **Text**
   - Value: `https://yoursite.com/github-repo-creator.html?name=[RepoName]&desc=[RepoDesc]&auto=true`
5. Add action: **URL Encode**
   - Mode: Encode
6. Add action: **Open URLs**
7. Name it "New Repo"

### Option C: Via Siri / Voice

Same as above, then:
1. Tap the shortcut name
2. Add to Siri
3. Record phrase: "New repo" or "Create repo"

Now say "Hey Siri, new repo" → speak name → done!

---

## Hosting on GitHub Pages (Free)

1. Create a new repo (e.g., `gh-tools`)
2. Upload `github-repo-creator.html` as `index.html`
3. Go to Settings → Pages
4. Source: Deploy from branch → main → / (root)
5. Save

Your URL will be: `https://yourusername.github.io/gh-tools/`

---

## Security Notes

- Your GitHub token is stored in localStorage on your device only
- Never shared with any server
- The app runs entirely client-side
- No analytics, no tracking

---

## Troubleshooting

**"Network error"**
- Check your internet connection
- Verify your token hasn't expired

**"Bad credentials"**
- Token may have expired or been revoked
- Generate a new token with `repo` scope

**"Repository creation failed"**
- Repo name may already exist
- Name must be lowercase, no spaces (use dashes)

**Auto-create not working**
- Make sure token is saved first (open app once manually)
- Check URL has `auto=true` parameter
