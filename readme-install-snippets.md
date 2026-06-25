# README install snippets (maintainers)

Copy into extension `README.md` when adding or refreshing install docs. Replace placeholders per extension.

| Placeholder | Example |
|-------------|---------|
| `{NPM_PACKAGE}` | `wappler-beegone-honeypot` |
| `{CATALOG_NAME}` | `BeeGone Honeypot` |
| `{ASSISTANT_CHOICE}` | `Both`, `Server Connect`, or `App Connect` |
| `{VERIFY_DESTS}` | `extensions/`, `lib/modules/`, and `public/` |

## Top blockquote — Server Connect only

```markdown
> ## ⚠️ NPM users — please read first (important)
>
> Mr Cheese extensions were built for **Git copy install** first. Wappler's **npm** lane (Project Settings → Extensions) puts the package in `node_modules` but **does not automatically copy** Server Connect modules into your project folders. **Project Updater alone is not enough** for these extensions.
>
> **If you use npm, follow the full [npm install](#npm-install-wappler-project-settings) section below.** Quick summary:
>
> 1. Add this extension in **Wappler → Project Settings → Extensions**, then run **`npm install`** in your project root.
> 2. **Verify** the package landed: `ls node_modules/{NPM_PACKAGE}/package.json` (if this fails, fix registration before copying anything).
> 3. Run the copy script from the **[Mr Cheese npm install assistant](https://www.mrcheese.co.uk/extensions/install/npm)** — choose **Server Connect** — into `extensions/` and `lib/modules/`.
> 4. **Quit Wappler completely** (including the tray icon) and reopen your project.
>
> Mr Cheese is working on a combined solution and has proposed **[`wappler-install.json`](https://github.com/MrCheeseGit/Wappler-Git-Extension-Manifest-Standard)** so install tools (and hopefully Wappler itself) can deploy extensions the same way from Git or npm. Until then, sorry for the extra steps — this is one reason these extensions were never intended to rely on npm alone.
>
> **Prefer Git?** Use the [Git Extension Installer](https://www.mrcheese.co.uk/extensions/install) — the most complete path, no npm required.
```

## Top blockquote — App Connect and/or Both

Add step 4 Project Updater and renumber restart to step 5. Use **Both** or **App Connect** in step 3 per extension.

## Installation intro (both paths)

```markdown
Pick **one** install path and follow it completely:

| Path | Best for |
|------|----------|
| **Git** (recommended) | Most reliable; uses `git clone` + copy from the repo |
| **npm** | You already use Wappler Project Settings → Extensions |

Both paths copy files into `{VERIFY_DESTS}`. The npm path also requires verifying `node_modules/{NPM_PACKAGE}` exists **before** you run any copy commands.
```

## npm install section

```markdown
### npm install (Wappler Project Settings)

Use this when you register the extension through **Wappler → Project Settings → Extensions**. The npm package makes the extension appear in Wappler but **does not copy runtime files** into your project folders.

1. **Register in Wappler** — Project Settings → Extensions → Add → enter `{NPM_PACKAGE}` or this repository's GitHub URL.
2. **Install dependencies** — from your Wappler project root (folder with `package.json`):
   ```bash
   npm install
   ```
3. **Verify before copying** (required):
   ```bash
   ls node_modules/{NPM_PACKAGE}/package.json
   ```
   If this command fails, stop here. The extension is missing from `.wappler/project.json` or `npm install` did not succeed. **Do not** run copy commands until `node_modules` contains the package.
4. **Copy files into your project** — open the **[npm install assistant](https://www.mrcheese.co.uk/extensions/install/npm)**, select **{CATALOG_NAME}**, choose **{ASSISTANT_CHOICE}**, copy the generated script, and run it from your project root.
5. (App Connect / Both only) Run **Project Updater → Update** after the copy script.
6. **Quit Wappler completely** (tray icon too) and reopen your project.

#### Local `file:` development (optional)

For extension authors testing from a folder on disk:

```json
"devDependencies": {
  "{NPM_PACKAGE}": "file:../path/to/this-extension"
}
```

After you change extension source, run `npm install {NPM_PACKAGE}` again in the project root. If the version in `package.json` did not change, remove the cached copy first: `rm -rf node_modules/{NPM_PACKAGE}` then `npm install {NPM_PACKAGE}`. Then run the npm install assistant copy script (or Git manual copy) and restart Wappler.
```

Server Connect only: omit step 5 and use step 5 for restart (renumber).
