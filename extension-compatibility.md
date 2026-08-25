# Mr Cheese Wappler extensions: compatibility

How our **public** extensions fit together in one Wappler project. Each extension also has its own README and `wappler-install.json`.

**Installer:** [mrcheese.co.uk/extensions/install](https://www.mrcheese.co.uk/extensions/install)  
**Catalog:** [mrcheese.co.uk/extensions](https://www.mrcheese.co.uk/extensions)

---

## Baseline (all extensions)

| Topic | Guidance |
|-------|----------|
| **Wappler** | Register the extension in Project Settings, run **npm install**, use Project Updater if you use a `file:` or npm package, then **quit Wappler completely** and reopen. The IDE reads `components.hjson` from `node_modules` after install. |
| **Node** | Server Connect extensions run in your project’s Node runtime (typical: Node 18+ or 20 LTS). |
| **Copy paths** | Many extensions copy the same `.js` to both `lib/modules/` and `extensions/server_connect/modules/`. Keep both in sync. |
| **Routes hooks** | Optional files under `extensions/server_connect/routes/` load at server start. Only one service worker per site scope for web push. |

---

## Standalone extensions

These work without other Mr Cheese extensions:

| Extension | Type | Notes |
|-----------|------|--------|
| [Wap-Lastic](https://github.com/MrCheeseGit/Wap-Lastic-Wappler-Elastic-Search-Extension) | Server Connect | Search scoring |
| [ClickSend SMS](https://github.com/MrCheeseGit/Wappler-ClickSend-SMS-Extension) | Server Connect | `CLICKSEND_*` env |
| [Generate Auth Code](https://github.com/MrCheeseGit/Wappler-Generate-Auth-Code-Extension) | Server Connect | OTP / tokens |
| [Cache Machine](https://github.com/MrCheeseGit/Wappler-Cache-Machine-Extension) | Server Connect | Optional Redis |
| [DataDump Export](https://github.com/MrCheeseGit/Wappler-Data-Dump-Export-PDF-CSV-Excel-Extension) | App Connect | Font Awesome on layout; pdfmake / SheetJS load from CDN unless self-hosted |
| [Great Range Picker](https://github.com/MrCheeseGit/greatRangePicker) | App Connect | Font Awesome on layout; Bootstrap 5 buttons; popover portaled to `document.body` |
| [Great Range Time Picker](https://github.com/MrCheeseGit/greatRangeTimePicker) | App Connect | Date + time range; optional presets (off by default); pairs with Great Range Picker when both are needed |

---

## Common pairs (optional, not required)

| Stack | Extensions | Why |
|-------|------------|-----|
| **SMS OTP** | Generate Auth Code + ClickSend SMS | Generate code in SC, send via ClickSend |
| **Rich text CMS** | TipWap + Server Connect sanitize | Editor in App Connect, `tipwap_sanitize` before insert |
| **Login hardening** | BeeGone Honeypot + Redirect-IT | Bot guard on form, redirect after API login |
| **Login notify** | PuSH-IT + Redirect-IT | Push on sign-in, then redirect (see below) |
| **Reports + bookings** | Great Range Picker + Great Range Time Picker | Date-only filters on dashboards; date + time ranges for events and availability |

---

## Redirect-IT

**Type:** Server Connect only.

### What it needs

| Piece | Required? |
|-------|-----------|
| `redirectit.js` + `redirectit.hjson` | Yes |
| `redirectit_nav.js` (routes hook) | Recommended (auto navigation after API success) |
| `session_json_flush.js` (routes hook) | Optional, recommended when login sets `req.session` and redirects in the same API call |
| App Connect **Browser** (`id="browser"`) | Only if you use custom `dmx-on:success` + `browser.goto` (not required for the nav hook) |

Drop the **Browser** component from the Wappler panel on your layout. Do not paste a bare `<div is="dmx-browser">` without the designer-generated scripts.

### Critical: step order in Server Actions

On **API** requests (forms, `dmx-serverconnect`, login JSON), **Redirect To Page** sets `redirectUrl` / `$redirect` and **stops the rest of that action**.

Put Redirect-IT **after** anything that must run on the same request, for example:

1. Auth, session, cookies  
2. **Push notifications**  
3. Audit log / webhooks / email  
4. **Redirect To Page** (last)

If redirect runs first, later steps (including PuSH-IT send) **do not run**.

### Login forms

- Either use the **nav hook** (`redirectit_nav.js`) **or** a custom Success handler that navigates. Avoid both firing on the same form.
- If the login form has a custom redirect handler (for example skipping the nav hook for that form id), document it in your project.
- Honeypot min-delay (BeeGone) is unrelated to Redirect-IT but can block fast double submits.

---

## PuSH-IT

**Type:** Server Connect + App Connect.

### Requirements

| Piece | Notes |
|-------|--------|
| `VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY`, `VAPID_SUBJECT` | Wappler Environment |
| `public/pushit_service_worker.js` | Required; register from subscribe flow, not a page `<script src>` |
| `dmx-pushit-subscribe` | App Connect component on settings or opt-in page |
| Subscribe / unsubscribe / status APIs | Your project implements storage (see extension examples) |

### Local development

Use one origin consistently (`http://localhost:PORT`). `127.0.0.1` is a different origin for push subscriptions.

### Login push

If you send push on login in the same API as Redirect-IT, the **push steps must come before Redirect-IT** (see above).

### Test send

Use the extension’s test API from the PuSH-IT examples. Confirm `sent: 1` in the API response before debugging the browser.

---

## BeeGone Honeypot

**Type:** Server Connect + App Connect.

- Place `dmx-beegone-honeypot` inside the form; validate with the Server Connect step before insert or login.
- **Min seconds** on the component delays valid submits. Users must wait before the first submit succeeds.

---

## TipWap

**Type:** Server Connect + App Connect.

- `dmx-tipwap-editor` syncs to a real `<textarea>` for Import From Form.
- Run **Sanitize HTML** (or excerpt / word count) in Server Connect before database insert.

---

## Pretty Good Database

**Type:** Server Connect + App Connect.

- **Mode Secure:** public key in env; private key unlocked in browser only.
- **Mode Simple:** optional server-side encrypt/decrypt via env keys (understand the tradeoff).
- See the extension README for unlock and field wiring. No cross-extension dependencies.

---

## DataDump Export

**Type:** App Connect only.

- Font Awesome (or compatible icons) on the layout.
- Place `dmx-datadump-export` **outside** the export target block.
- PDF uses lazy-loaded pdfmake; Excel uses SheetJS (CDN URLs configurable per instance).

---

## Great Range Picker

**Type:** App Connect only.

| Piece | Notes |
|-------|--------|
| `dmx-great-range-picker` | Preset sidebar, dual calendar, full-width trigger |
| Font Awesome | Chevron icons on trigger and month navigation |
| Bootstrap 5 | Apply / Cancel use `btn`, `btn-primary`, `btn-link` |
| `default-preset` | Initial range when Start/End date fields are empty (Today, Last 30 days, Next 30 days, YTD, etc.) |
| `display-format` | Trigger summary only: `DD/MM/YYYY`, `DD-MM-YYYY`, `MM/DD/YYYY`, `YYYY-MM-DD`, or locale default. Native Start/End date inputs follow the browser. |
| `blocked-dates` | Bind unavailable `YYYY-MM-DD` values (comma-separated or JSON array). Days disabled in calendar; Apply rejected if range spans a blocked day. |
| `color-scheme` | `dark`, `light`, or `auto` (system preference). Theme applies to trigger and body-portaled popover. Use **light** on dark host pages; the picker resets Bootstrap tokens inside its scope. |
| `placement` | `modal` centres popover in nearest Bootstrap dialog; `trigger` anchors below the button |
| `timezone` | IANA zone for Today and preset boundaries (e.g. `Europe/Lisbon`) |

Popover is moved to `document.body` when open so modals and `overflow: hidden` panels do not clip it. Bind `{{component.data.dateFrom}}`, `{{component.data.dateTo}}`, and `{{component.data.preset}}`, or handle `dmx-on:changed`.

---

## Great Range Time Picker

**Type:** App Connect only. Sibling to [Great Range Picker](#great-range-picker): use this when you need **start/end times** (bookings, events, availability). Preset sidebar is **off by default**; enable **Show preset sidebar** for report-style filters.

| Piece | Notes |
|-------|--------|
| `dmx-great-range-time-picker` | Start/end date + time fields, dual calendar, full-width trigger |
| Font Awesome | Chevron icons on trigger and month navigation |
| Bootstrap 5 | Apply / Cancel use `btn`, `btn-primary`, `btn-link` |
| `time-from` / `time-to` | 24-hour `HH:MM`; bind `{{component.data.timeFrom}}` and `{{component.data.timeTo}}` |
| `default-time-from` / `default-time-to` | Defaults when times are empty (`00:00` / `23:59`) |
| `time-step` | Native time input step in seconds (default `900` = 15 minutes) |
| `show-presets` | Optional preset sidebar (default off) |
| `default-preset` | Initial date range when Start/End date fields are empty (`custom` uses today through +7 days) |
| `display-format` | Trigger summary dates only; times always show as `HH:MM` on the trigger |
| `blocked-dates` | Bind unavailable `YYYY-MM-DD` values (comma-separated or JSON array) |
| `color-scheme` | `dark`, `light`, or `auto` |
| `placement` | `modal` centres popover in nearest Bootstrap dialog; `trigger` anchors below the button |
| `timezone` | IANA zone for Today and preset boundaries |

Bind `{{component.data.dateFrom}}`, `{{component.data.dateTo}}`, `{{component.data.timeFrom}}`, `{{component.data.timeTo}}`, and `{{component.data.preset}}`, or handle `dmx-on:changed` (detail includes all five).

---

## Troubleshooting quick reference

| Symptom | Likely cause |
|---------|----------------|
| API login works but browser never moves | Missing Redirect-IT nav hook or missing `redirectUrl` in JSON response |
| Session lost right after login redirect | Install `session_json_flush.js` |
| Login push stopped after adding Redirect-IT | Push steps are below redirect in the Server Action; move push above redirect |
| Test push works, login push does not | Same as above, or no active row for that `user_uuid` in your subscriptions table |
| Subscribe UI errors on status API | Update `dmx-pushit-subscribe` to latest; status API must return `findActive.active` or `subscribed` |
| Component missing in Wappler picker | `npm install`, Project Updater, quit Wappler fully, reopen |
| Great Range Picker light mode still looks dark | Set **Color scheme** to `light` (not `auto` on a dark OS). Host `color-scheme: dark` on date inputs is overridden inside the picker in v1.0.0+. |
| Great Range Picker Start/End show wrong date pattern | Native `<input type="date">` format is browser-controlled. Use **Display format** for the trigger button only (v1.1.0+). |
| Great Range Picker Apply blocked | Range includes a **Blocked dates** day; narrow the range or refresh the bound date list from your query. |
| Great Range Time Picker times missing on Apply | Set **Start time** / **End time** or configure **Default start time** / **Default end time** (`00:00` / `23:59` by default). |
| Great Range Time Picker preset sidebar missing | **Show preset sidebar** defaults off; enable it in component properties for report-style filters. |
| Service worker 404 | Copy `pushit_service_worker.js` to `public/` and redeploy |

---

## Extension index

| Extension | GitHub |
|-----------|--------|
| PuSH-IT | [Wappler-PuSH-IT-Extension](https://github.com/MrCheeseGit/Wappler-PuSH-IT-Extension) |
| Redirect-IT | [Wappler-Redirect-IT-Extension](https://github.com/MrCheeseGit/Wappler-Redirect-IT-Extension) |
| ClickSend SMS | [Wappler-ClickSend-SMS-Extension](https://github.com/MrCheeseGit/Wappler-ClickSend-SMS-Extension) |
| Generate Auth Code | [Wappler-Generate-Auth-Code-Extension](https://github.com/MrCheeseGit/Wappler-Generate-Auth-Code-Extension) |
| Cache Machine | [Wappler-Cache-Machine-Extension](https://github.com/MrCheeseGit/Wappler-Cache-Machine-Extension) |
| BeeGone Honeypot | [Wappler-BeeGone-Honeypot-Extension](https://github.com/MrCheeseGit/Wappler-BeeGone-Honeypot-Extension) |
| TipWap | [Wappler-TipWap-Tiptap-Extension](https://github.com/MrCheeseGit/Wappler-TipWap-Extension) |
| Pretty Good Database | [Wappler-Pretty-Good-Database-Extension](https://github.com/MrCheeseGit/Wappler-Pretty-Good-Database-Extension) |
| DataDump Export | [Wappler-Data-Dump-Export-PDF-CSV-Excel-Extension](https://github.com/MrCheeseGit/Wappler-Data-Dump-Export-PDF-CSV-Excel-Extension) |
| Great Range Picker | [greatRangePicker](https://github.com/MrCheeseGit/greatRangePicker) |
| Great Range Time Picker | [greatRangeTimePicker](https://github.com/MrCheeseGit/greatRangeTimePicker) |
| Wap-Lastic | [Wap-Lastic-Wappler-Elastic-Search-Extension](https://github.com/MrCheeseGit/Wap-Lastic-Wappler-Elastic-Search-Extension) |

---

## Changelog for this document

| Date | Change |
|------|--------|
| 2026-08-25 | Great Range Time Picker v0.1.0: date + time range, optional presets, blocked dates |
| 2026-08-25 | Great Range Picker v1.1.0: display format (trigger), blocked dates for bookings/events |
| 2026-08-24 | Great Range Picker v1.0.0 docs: default date range, color scheme (incl. light on dark hosts) |
| 2026-08-24 | Added Great Range Picker (standalone App Connect; default date range, color scheme, modal-safe popover) |
| 2026-06-20 | Initial public compatibility guide (Redirect-IT step order, PuSH-IT login push, baseline table) |
