# README install snippets (maintainers)

Copy into extension `README.md` when adding or refreshing install docs. Replace placeholders per extension.

| Placeholder | Example |
|-------------|---------|
| `{NPM_PACKAGE}` | `wappler-beegone-honeypot` |
| `{VERIFY_DESTS}` | `extensions/`, `lib/modules/`, and `public/` |

## Installation intro

```markdown
## Installation

| Path | |
|------|--|
| **npm** | Wappler Project Settings → Extensions (`{NPM_PACKAGE}`) |
| **Git** | [Extension Installer](https://www.mrcheese.co.uk/extensions/install) or manual copy below |

Git manual copy installs into `{VERIFY_DESTS}`.
```

## npm install section — Server Connect only

```markdown
### npm install (Wappler Project Settings)

1. **Wappler** → Project Settings → Extensions → Add → `{NPM_PACKAGE}`
2. From your project root: `npm install`
3. **Quit Wappler completely** and reopen your project.
```

## npm install section — App Connect / Both

Add Project Updater before the restart step:

```markdown
3. Run **Project Updater → Update** when prompted.
4. **Quit Wappler completely** and reopen your project.
```

## Git manual install

Use `wappler-install.json` copy paths. Sources are under `server_connect/modules/` and `includes/` in the repo. See the [Extension Installer](https://www.mrcheese.co.uk/extensions/install).

Optional fallback: [npm sync assistant](https://www.mrcheese.co.uk/extensions/install/npm) copies from `node_modules` if Project Updater did not deploy every file.
