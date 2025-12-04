# GitHub Repo Creator - Project Context for Claude Code

## What This Is

A mobile-first PWA that lets you quickly create GitHub repositories from your phone. Designed to be installed as a home screen app on iOS and integrated with Apple Shortcuts for hands-free repo creation.

**Live URL**: https://smhunt.github.io/gh-repo-creator/
**Repo**: https://github.com/smhunt/gh-repo-creator

## Core Features

1. **Quick Repo Creation** - Enter name + optional description, tap create
2. **Apple Shortcuts Integration** - URL parameters allow automation:
   - `?name=repo-name` - Pre-fill repo name
   - `?desc=Description` - Pre-fill description  
   - `?private=true|false` - Set visibility
   - `?auto=true` - Auto-create on load (for Shortcuts)
3. **PWA Support** - Installable on iOS home screen with proper icons/theme
4. **Update Notifications** - App checks for new versions and prompts user to reload
5. **Settings Panel** - Token management, reset, copy base URL for Shortcuts

## Tech Stack

- Single HTML file (self-contained)
- React 18 via CDN (unpkg)
- Babel standalone for JSX transformation
- No build step required
- GitHub Pages for hosting

## Current Bug: Credentials Not Persisting

### Problem
The GitHub Personal Access Token is not being saved/persisted between app sessions on iOS. User enters token, it works for creating repos, but on next app launch it's gone.

### What We've Tried
- Basic localStorage with `gh_pat` key
- Added try/catch error handling
- Added verification that save succeeded
- Auto-save token on successful repo creation
- Added reset button to clear storage

### Suspected Causes
1. **iOS PWA localStorage isolation** - Standalone PWAs may have different storage than Safari
2. **ITP (Intelligent Tracking Prevention)** - Safari may be clearing storage
3. **Storage quota issues** - Though unlikely for a small token string
4. **HTTPS/mixed content** - GitHub Pages is HTTPS so this shouldn't be it

### Potential Fixes to Try
1. **IndexedDB instead of localStorage** - More persistent on iOS
2. **Service Worker with Cache API** - Store token in SW scope
3. **Cookie-based storage** - As fallback
4. **Prompt to re-enter each session** - Accept the limitation, optimize UX for it
5. **Check Safari settings** - Settings → Safari → Advanced → Website Data

### Code Location
The token handling is in the `<script type="text/babel">` section:
- `saveToken()` function - saves to localStorage
- `useEffect` that loads token on mount
- `handleCreate()` - auto-saves on successful creation

## File Structure

```
/
├── index.html      # The entire app (single file)
├── README.md       # User-facing setup instructions
└── CLAUDE.md       # This file - dev context
```

## How to Update

1. Edit `index.html`
2. Bump version in `<meta name="app-version" content="X.X.X">`
3. Commit and push to `main` branch
4. GitHub Pages auto-deploys in ~30 seconds
5. Users see "New version available" banner

## Apple Shortcut Setup

For reference, here's how users set up the Shortcut:

```
1. Shortcuts app → + → Add actions:
2. "Ask for Input" → Prompt: "Project name"
3. "URL" → https://smhunt.github.io/gh-repo-creator/?name=[Provided Input]&auto=true
4. "Open URLs"
5. Name it "New Repo"
```

## GitHub Token Requirements

The app needs a GitHub Personal Access Token with `repo` scope:
- Classic token: Just needs `repo` checked
- Fine-grained: Needs Repository permissions → Administration + Contents (Read and write)

Create at: https://github.com/settings/tokens/new

## Development Notes

- The app uses React hooks (useState, useEffect, useRef)
- All components are inline in the single HTML file
- CSS is in a `<style>` block, not external
- Icons are inline SVGs as React components
- Dark theme with emerald accent color (#10b981)
- Space Grotesk + JetBrains Mono fonts from Google Fonts

## Next Steps / Ideas

- [ ] Fix credential persistence (priority)
- [ ] Add org/team selection
- [ ] Add license template picker
- [ ] Add .gitignore template selection
- [ ] Show recent repos created
- [ ] Haptic feedback on iOS
- [ ] Better offline support with Service Worker
