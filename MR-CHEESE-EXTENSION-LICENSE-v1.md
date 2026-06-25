# Mr Cheese Extension License v1.0 (draft for review)

**Status:** Adopted (v1.0) — applies to Mr Cheese Wappler extensions and Wappler Deployment Pipeline (WDP) across extension repositories  
**Copyright holder:** Mr Cheese (https://www.mrcheese.co.uk)  
**Permission requests:** info@mrcheese.co.uk  
**Suggested SPDX identifier:** `LicenseRef-MrCheese-Extension-1.0`  
**Suggested `package.json` value:** `"license": "SEE LICENSE IN LICENSE"`

> **Note:** This is a practical draft for your review, not legal advice. If you adopt it across a commercial product line, consider having a solicitor review the final text for your jurisdiction.

---

## Plain English summary

| You **may** | You **may not** (without written permission) |
|-------------|-----------------------------------------------|
| Download from official Mr Cheese channels (GitHub, npm, mrcheese.co.uk) | Redistribute, republish, or mirror the Software as your own extension or product |
| Install and use in any number of Wappler projects (personal or commercial) | Sell, sublicense, or license the Software or a derivative extension to others |
| Copy files into your project (`extensions/`, `lib/modules/`, `public/`, etc.) | Publish the Software (or a modified version) for others to download as a standalone package |
| Modify the Software for your own projects and deploy to production | Remove or alter copyright and license notices in distributed copies |
| Keep backups and internal copies for your own use | Use the **Mr Cheese** name or branding to imply your fork is official or endorsed |

To redistribute, OEM, embed in a product you sell, or otherwise transfer the Software to third parties, email **info@mrcheese.co.uk**.

---

## Full license text

Copy everything below the line into each extension repository as `LICENSE` when you adopt v1.

---

```
Mr Cheese Extension License v1.0

Copyright (c) 2026 Mr Cheese (https://www.mrcheese.co.uk)

1. Definitions

   "Software" means the source code, compiled assets, documentation, install
   manifests (including wappler-install.json), examples, and any other files
   distributed under this license as a Mr Cheese Wappler extension.

   "You" means the individual or legal entity exercising permissions granted
   by this license.

   "Authorized Distribution" means distribution of the Software by Mr Cheese
   through channels operated or designated by the copyright holder, including
   but not limited to:
     - GitHub repositories under the MrCheeseGit organization (or successor)
     - npm packages published under the mrcheesenpm account (or successor)
     - Downloads or installers provided at https://www.mrcheese.co.uk

   "Derivative Extension" means any work based on the Software that is offered
   or made available to others as a Wappler extension, npm package, Git
   repository, marketplace listing, or similar reusable product, whether or
   not modified.

2. Grant of license

   Permission is hereby granted, free of charge, to any person obtaining a copy
   of the Software through Authorized Distribution, to:

     (a) use and execute the Software for any purpose, including commercial
         use, within Your own projects;

     (b) modify the Software for Your own use;

     (c) install the Software into Your Wappler project directories (including
         extensions/, lib/modules/, public/, and equivalent paths) and deploy
         those projects to any environment You control; and

     (d) make reasonable copies for backup and internal development.

   This grant is personal to You and does not include any right to transfer
   these permissions to others except as part of a permitted deployment of Your
   own application that incorporates the Software.

3. Restrictions

   Without prior written permission from the copyright holder
   (info@mrcheese.co.uk), You may not:

     (a) redistribute, publish, or make available the Software, in whole or in
         part, to any third party;

     (b) sell, sublicense, lease, or otherwise commercialize the Software or
         any Derivative Extension;

     (c) distribute the Software or a Derivative Extension through npm, Git,
         a marketplace, an installer, or any other channel not controlled by
         Mr Cheese as Authorized Distribution;

     (d) remove or alter copyright, attribution, or license notices in copies
         of the Software You are permitted to hold; or

     (e) use the name "Mr Cheese", related logos, or branding in a way that
         suggests Your Derivative Extension is official, endorsed, or
         maintained by Mr Cheese.

   Nothing in this section prevents You from distributing Your own application
   (website, API, mobile app, or deployed Wappler project) that merely
   incorporates installed copies of the Software, provided You do not
   separately distribute the extension files themselves as a reusable product.

4. Permission requests

   Requests for redistribution, OEM licensing, white-label use, or other uses
   not expressly granted above may be submitted in writing to:

     info@mrcheese.co.uk

   The copyright holder may grant or deny such requests at its sole discretion.

5. No trademark license

   This license does not grant any rights to use trademarks, service marks, or
   trade names of Mr Cheese except as reasonably necessary to describe the
   origin of unmodified copies obtained through Authorized Distribution.

6. Disclaimer of warranty

   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
   FITNESS FOR A PARTICULAR PURPOSE, AND NONINFRINGEMENT.

7. Limitation of liability

   IN NO EVENT SHALL THE COPYRIGHT HOLDER BE LIABLE FOR ANY CLAIM, DAMAGES, OR
   OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT, OR OTHERWISE,
   ARISING FROM, OUT OF, OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
   OTHER DEALINGS IN THE SOFTWARE.

8. Termination

   This license terminates automatically if You breach Section 3. Upon
   termination, You must cease use and delete copies of the Software in Your
   possession, except that copies embedded in deployed applications may remain
   in place until You replace them with a lawful alternative.

9. Prior versions

   Releases of Mr Cheese extensions distributed under the MIT License before
   adoption of this license remain available under MIT at the version tag
   under which they were published. This license applies only to versions
   explicitly distributed with this license text.
```

---

## Adoption checklist (when you are ready)

1. Replace `LICENSE` in each extension repo with the full text above (plain text file, no markdown fences).
2. Update `package.json`: `"license": "SEE LICENSE IN LICENSE"`.
3. Update README: remove MIT badge; add **License** section linking to `LICENSE`.
4. Bump version and note license change in `CHANGELOG.md`.
5. Publish new versions to GitHub and npm (older MIT tags remain MIT per Section 9).
6. Optional: add `LICENSE-CHANGE.md` on mrcheese.co.uk explaining the change for existing users.

## Open questions for your review

1. **Public app repos** — Section 3 allows deploying apps that include the extension; confirm that matches your intent for clients who commit `extensions/` to GitHub.
2. **Agencies** — Should agencies installing on client projects be explicitly called out as permitted? (Currently covered by "your own projects" but could be clearer.)
3. **Contributions** — If you accept PRs on GitHub, add a contributor agreement or CLA clause (not included in v1).
4. **PHP BETA and manifest standard** — Same license, or docs-only repos under a simpler terms page?
