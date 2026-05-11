# English documentation

For the Persian originals, see the root [`README.md`](../README.md) and [`help.md`](../help.md).

- [README (English)](#readme-english)
- [Repository GitHub Actions guide (English)](#repository-github-actions-guide-english)

---

## README (English)

### GitBrowse

**This project and its scripts are dedicated to the honorable people of Iran.**

This repository was created so that people who, under restricted network conditions (including **national network** policies or filtering), do not have direct access to the open internet and global resources, can run a browser on **GitHub Actions** infrastructure (outside your restricted network), open web pages, save them, and receive **offline archives** (HTML, MHTML, assets, and network reports) in the same repository. It also supports collecting and testing **proxies** and routing the browser through a proxy to reach target sites.

> This tool is a “bridge” between you and the internet via GitHub. Legal, ethical, and security responsibility for correct use rests with you. Do not use anonymous proxies or a public repository for logins, payments, or private data.

This repository is a **fork** and continuation of [nikzad-avasam/downloader](https://github.com/nikzad-avasam/downloader).

---

### GitHub Actions overview

| Name in GitHub Actions | Workflow file | Short role |
|------------------------|---------------|------------|
| Browse the Web | [`.github/workflows/browser.yml`](../.github/workflows/browser.yml) | Real browser session, offline output, proxy, automation |
| Download from url | [`.github/workflows/downloader.yaml`](../.github/workflows/downloader.yaml) | Download files from URLs into `downloads/` |
| Collect Free Proxies | [`.github/workflows/proxy-list.yml`](../.github/workflows/proxy-list.yml) | Collect and test free proxies → `proxy-list.json` |
| Clean generated folders | [`.github/workflows/cleaner.yml`](../.github/workflows/cleaner.yml) | Remove `downloads/`, `pages/`, `sessions/`, and `browse.md` |

Step-by-step guide for each workflow: [`help.md`](../help.md) (Persian) — English version: [below](#repository-github-actions-guide-english).

---

### Features (via GitHub Actions)

#### [Browse the Web](../.github/workflows/browser.yml)

Runs **Chromium + Playwright** on Ubuntu, visits a URL, and saves output into the repository:

| Topic | Short description |
|--------|-------------------|
| Main input | Page address (`url`) |
| Automation | JSON steps: clicks, form fill, waits, scroll, etc. (`automation`) — example: [`browser-configs/automation.example.json`](../browser-configs/automation.example.json) |
| Pop-ups / cookies | JSON rules for modals (`popup_rules`) — example: [`browser-configs/popup-rules.example.json`](../browser-configs/popup-rules.example.json) |
| Session | Optional session name; cookie/storage reuse in `sessions/` (`session_key`, `persist_session`) |
| After load | Seconds to wait before capture (`wait_after_load`) |
| Auto-scroll | For lazy-loaded images and content (`auto_scroll`) |
| Network | Save text response bodies (`save_response_bodies`, size limits) |
| Assets | Max size per file; optional skip for video (`max_asset_size_mb`, `include_videos`) |
| Privacy in logs | Redact sensitive headers (`redact_sensitive`) |
| Proxy | Modes `none`, `manual`, `fastest-from-file`, `rank-from-file`, `random-from-file` + list file (`proxy_mode`, `proxy_list_file`, …) and optional secrets `PROXY_SERVER` / `PROXY_USERNAME` / `PROXY_PASSWORD` |

Output usually includes `pages/`, `browse.md`, optionally `sessions/`, a ZIP archive (split if large), and an **Artifact** on the same run. Runner script: [`scripts/browser_capture.py`](../scripts/browser_capture.py).

#### [Download from url](../.github/workflows/downloader.yaml)

Downloads one or more files; **space-separated** URLs on the GitHub runner; output under `downloads/` and updates `downloads/README.md`. **normal** mode (each file in its own folder, multi-part ZIP ~90 MB when large) or **zip** (single archive of all successful downloads). Optional **password** for encrypted ZIP. Note: this workflow may append a section to the **root README** listing downloads.

#### [Collect Free Proxies](../.github/workflows/proxy-list.yml)

Collects candidates from free proxy sources, tests in parallel, ranks by speed/health, and writes:

- `proxy-list.md` — human-readable table  
- `proxy-list.json` — for the browser workflow  
- `proxy-list.env` — quick environment variables  

Configurable: count, protocol (`http` / `https` / `socks4` / `socks5` / `all`), test URL (for proxies suited to a specific site), timeout, concurrency, max candidates, and custom sources file (`proxy-sources.example.txt` in the root if needed).

#### [Clean generated folders](../.github/workflows/cleaner.yml)

Removes generated data: `downloads/`, `pages/`, `sessions/`, and `browse.md`, and restores the base layout (to slim the repo or start clean).

---

### Quick start

1. **Fork** this repository (or use a copy you trust).  
2. Open **Actions** on GitHub and run the desired workflow with **Run workflow**.  
3. **Web page (offline + network):** open [**Browse the Web**](../.github/workflows/browser.yml), fill **url**, and run.  
4. For browsing via proxy: run **Collect Free Proxies** first, then in **Browse the Web** pick the proxy mode and list file; or configure manual proxy/secrets per the [proxy guide](proxy-setup.md).

---

### Documentation (`docs`)

| Document | Topic |
|----------|--------|
| [Proxy and Actions](proxy-setup.md) | Proxy modes, GitHub secrets, proxy list integration |
| [Browser automation in Actions](browser-automation.md) | Playwright, offline output, session, network, security |
| [JSON reference for automation and pop-ups](browser-json-reference.md) | Structure of `automation` and `popup_rules` |

---

### Browser sample configs (`browser-configs`)

| File | Purpose |
|------|---------|
| [`browser-configs/automation.example.json`](../browser-configs/automation.example.json) | Sample automation steps for the browser workflow |
| [`browser-configs/popup-rules.example.json`](../browser-configs/popup-rules.example.json) | Sample pop-up / cookie rules |

---

### Local dependencies (running scripts on your machine)

[`requirements-browser.txt`](../requirements-browser.txt) and [`scripts/`](../scripts/) mirror the CI logic; details are in the docs above.

---

### Security reminder

Free proxies are for **testing**, not sensitive logins or payments. `sessions/` may contain tokens and cookies; on **public** repos, use `persist_session` carefully and enable `redact_sensitive` when needed.

---

### License

Code in this repository is released under the [MIT License](../LICENSE).

---

*If this README or workflow names diverge from files under `.github/workflows`, the YAML filenames are the source of truth.*

---

## Repository GitHub Actions guide (English)

This section is the English counterpart of [`help.md`](../help.md): what each **workflow** does and how to run it.

### Before you start (common)

1. **Fork** the repository (or enable **Actions** on a repo you control).  
2. In GitHub, open **Actions**.  
3. Select the workflow name in the left column.  
4. Click **Run workflow**, fill inputs, confirm **Run workflow**.  
5. Open the run for logs and output; download **Artifacts** at the bottom of the run page when present.

For browser, proxy, and JSON automation, see also [`docs/`](.):

| File | Topic |
|------|--------|
| [docs/proxy-setup.md](proxy-setup.md) | Proxies in Actions and `proxy-list.json` |
| [docs/browser-automation.md](browser-automation.md) | Capture, session, network features |
| [docs/browser-json-reference.md](browser-json-reference.md) | `automation` and `popup_rules` structure |

Sample JSON: [browser-configs/automation.example.json](../browser-configs/automation.example.json) and [browser-configs/popup-rules.example.json](../browser-configs/popup-rules.example.json).

---

### 1. Download from url

**Workflow file:** [`.github/workflows/downloader.yaml`](../.github/workflows/downloader.yaml)

#### What it does

Downloads one or more files from the internet **on the GitHub runner** (GitHub’s network, not your home IP) and stores results under `downloads/` in this repo, with a descriptive `README.md` per folder and an updated index in `downloads/README.md`. Large files may be split into multiple ZIP parts (~90 MB limit). Uses `aria2c`, with **curl** as fallback.

> Note: this workflow may add a “downloaded files in your GitHub repo” section to the **root README.md**. If you maintain README manually, review `git diff` after a run.

#### Inputs

| Input | Description |
|--------|-------------|
| **urls** | Required. One or more URLs **space-separated** (not comma). Example: `https://example.com/a.zip https://example.com/b.iso` |
| **mode** | `normal`: each file zipped in its own subfolder (or multi-part if large). `zip`: all successful files in **one** time-stamped archive. |
| **password** | Optional. If set, ZIP is password-protected (`zip -P`). |

**Notes:**

- Filenames come from the URL `basename`; invalid characters are sanitized.  
- On failure, an error report is written to `downloads/<folder>/README.md`.  
- Commits are pushed in **batches of five** files to stay within size limits.

---

### 2. Browse the Web

**Workflow file:** [`.github/workflows/browser.yml`](../.github/workflows/browser.yml)

#### What it does

Opens a **URL** in a real browser (**Chromium + Playwright + Python**) on GitHub’s runner, optional automation, pop-up/cookie handling, **offline** page bundle, network log, and optional **proxy** routing. Output is usually committed under `pages/`, `browse.md`, and optionally `sessions/`; plus **Artifacts** and ZIP or split ZIP (~50 MB per part).

#### Inputs

| Input | Default / type | Description |
|--------|----------------|-------------|
| **url** | Required | Page to open. |
| **automation** | `[]` | JSON array of steps (click, wait, fill, …). Example: `browser-configs/automation.example.json`. |
| **popup_rules** | `[]` | JSON rules for cookie/consent/modals; example: `browser-configs/popup-rules.example.json`. |
| **session_key** | empty | Session filename; if empty, derived from domain (`sessions/<domain>.json`). |
| **wait_after_load** | `2` | Seconds to wait after load and after automation before capture. |
| **auto_scroll** | `true` | Scroll to trigger lazy loading. |
| **persist_session** | `true` | Save/reuse cookies and storage in the repo. Use caution on public repos. |
| **save_response_bodies** | `true` | Save bodies of textual responses (Ajax, etc.). |
| **max_response_body_mb** | `2` | Max size per saved body. |
| **max_asset_size_mb** | `25` | Max size per offline asset. |
| **include_videos** | `false` | Download videos (usually leave off). |
| **redact_sensitive** | `false` | Redact sensitive headers in network logs. |
| **proxy_mode** | `none` | One of: `none`, `manual`, `fastest-from-file`, `rank-from-file`, `random-from-file`. |
| **proxy_server** | empty | For `manual`: e.g. `http://IP:PORT` or `socks5://…`. Empty + manual → secret `PROXY_SERVER`. |
| **proxy_username** / **proxy_password** | empty | Prefer secrets `PROXY_USERNAME` and `PROXY_PASSWORD`. |
| **proxy_list_file** | `proxy-list.json` | Proxy list file (output of the proxy collection workflow). |
| **proxy_list_rank** | `1` | When `rank-from-file`: proxy rank in the JSON file. |
| **proxy_allow_direct_fallback** | `false` | If the proxy fails, continue without proxy or not. |

Details: [docs/proxy-setup.md](proxy-setup.md) and [docs/browser-automation.md](browser-automation.md).

---

### 3. Collect Free Proxies

**Workflow file:** [`.github/workflows/proxy-list.yml`](../.github/workflows/proxy-list.yml)

#### What it does

Collects proxy candidates from **free** sources, tests in parallel, keeps the fastest, and writes:

- `proxy-list.md` (readable table, also summarized in the run),  
- `proxy-list.json` (for the browser workflow),  
- `proxy-list.env`.

#### Inputs

| Input | Default | Description |
|--------|---------|-------------|
| **count** | `10` | How many top proxies to keep. |
| **protocol** | `http` | `http`, `https`, `socks4`, `socks5`, or `all`. |
| **test_url** | ipify | URL used to test latency/health; use a site-specific URL if needed. |
| **timeout_seconds** | `8` | Per-request test timeout. |
| **concurrency** | `80` | Parallel tests. |
| **max_candidates** | `1200` | Max candidates before testing. |
| **sources_file** | empty | Custom sources file path; empty = built-in sources. Example: `proxy-sources.example.txt`. |

After success, in **Browse the Web** you can set e.g. `proxy_mode` to `fastest-from-file` and keep `proxy_list_file` as `proxy-list.json`.

---

### 4. Clean generated folders

**Workflow file:** [`.github/workflows/cleaner.yml`](../.github/workflows/cleaner.yml)

#### What it does

**Permanently removes** generated data below and recreates empty folders:

- `downloads/`  
- `pages/`  
- `sessions/`  
- `browse.md`  

Useful to slim the repo or reset before a new capture or download.

#### Inputs

| Input | Description |
|--------|-------------|
| **warning** | Text reminder only; does not affect logic. Know that **folders and browse.md will be deleted**. |

Run the workflow; if anything changed, it commits and pushes to the current branch.

---

### Suggested order (common scenarios)

| Goal | Suggested order |
|------|-----------------|
| Save a web page offline only | **Browse the Web** → **Clean** if the repo grows |
| Download large or multiple direct files | **Download from url** |
| Sites reachable from GitHub’s IP but not from Iran | **Browse**; optionally **Collect Free Proxies** then **Browse** with a suitable `proxy_mode` |
| Clear all generated outputs | **Clean generated folders** |

If workflow behavior changes, treat the workflow YAML as canonical; this English guide summarizes current behavior.
