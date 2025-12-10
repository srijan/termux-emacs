# Termux Emacs

Termux APKs resigned with the Emacs Android keystore for compatibility with [Emacs on Android](https://www.gnu.org/software/emacs/).

## Why?

Emacs for Android uses a specific signing key. To install Termux alongside Emacs and allow them to work together, all APKs need to be signed with the same key. This repository automatically mirrors official Termux releases, resigned with the Emacs keystore.

## Included Apps

| App | Upstream Repository |
|-----|---------------------|
| Termux | [termux/termux-app](https://github.com/termux/termux-app) |
| Termux:API | [termux/termux-api](https://github.com/termux/termux-api) |
| Termux:Boot | [termux/termux-boot](https://github.com/termux/termux-boot) |
| Termux:Float | [termux/termux-float](https://github.com/termux/termux-float) |
| Termux:Styling | [termux/termux-styling](https://github.com/termux/termux-styling) |
| Termux:Widget | [termux/termux-widget](https://github.com/termux/termux-widget) |

## Installation

1. Go to [Releases](../../releases)
2. Download the APKs for your device architecture (arm64-v8a for most modern devices)
3. Install the APKs

## How It Works

A GitHub Actions workflow runs weekly to:
1. Check each upstream Termux repo for new releases
2. Download the official APKs
3. Remove the original signature
4. Resign with the [Emacs keystore](https://cgit.git.savannah.gnu.org/cgit/emacs.git/plain/java/emacs.keystore)
5. Create a mirrored release with the resigned APKs

## Release Naming

Releases are tagged as `{app}-{version}`:
- `termux-app-v0.118.3`
- `termux-api-v0.53.0`

## Signing Key

APKs are signed with the public Emacs debug keystore:
- **Keystore:** [`keys/emacs.keystore`](keys/emacs.keystore)
- **Source:** [cgit.git.savannah.gnu.org](https://cgit.git.savannah.gnu.org/cgit/emacs.git/plain/java/emacs.keystore)
- **Password:** `emacs`

## License

MIT
