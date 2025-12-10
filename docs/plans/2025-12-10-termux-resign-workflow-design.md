# Termux APK Resign Workflow Design

## Overview

A GitHub Actions workflow that automatically syncs Termux APKs from official repos, resigns them with Emacs keystore, and creates mirrored releases.

## Monitored Repos

- termux/termux-app
- termux/termux-api
- termux/termux-boot
- termux/termux-float
- termux/termux-styling
- termux/termux-widget
- termux/termux-x11

## Triggers

- **Scheduled:** Weekly (Sundays at midnight UTC)
- **Manual:** workflow_dispatch with optional repo filter and force flag

## Release Mirroring

Each upstream release becomes a release in this repo with prefixed tags:
- `termux-app-v0.118.3`
- `termux-api-v0.53.0`
- `termux-x11-nightly-20251130`

## Workflow Structure

### Job 1: check-releases
- Matrix strategy for all 7 repos
- Checks GitHub API for latest release
- Compares against existing tags in this repo
- Outputs JSON of repos with new releases

### Job 2: process-release
- Runs only for repos with new releases
- Dynamic matrix from check-releases output
- Steps: download APKs, resign, create release

## APK Processing Pipeline

1. Download all APK assets from upstream release
2. For each APK:
   - Remove existing signature (META-INF/)
   - Align with zipalign
   - Sign with apksigner using Emacs keystore
3. Create release with same tag (prefixed)
4. Upload resigned APKs
5. Copy release notes from upstream

## Signing Configuration

- Keystore: `keys/emacs.keystore` (committed to repo)
- Password: `emacs` (public debug keystore)
- Key alias: `emacs`

## Error Handling

- Skip releases with no APKs
- Fail job if signing fails (other repos continue)
- `force` flag allows reprocessing existing releases

## File Structure

```
termux-emacs/
├── .github/workflows/sync-termux.yml
├── keys/emacs.keystore
├── LICENSE
└── README.md
```
