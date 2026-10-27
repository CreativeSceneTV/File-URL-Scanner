# File & URL Scanner

A static web page that inspects files and links **entirely in your browser**. Nothing is uploaded, and there is no backend, build step or API key.

> **Not antivirus.** Results are heuristics, not proof of malware, and a clean result is not a guarantee. Only scan files you own or are allowed to analyze.

## What it does

**Files** (any type, any number, up to 100 MB each)
- Detects the real type from about 40 file signatures and compares it with the extension
- Flags double extensions, right-to-left override characters and trailing-space tricks
- PDF: JavaScript, auto-run actions, `/Launch`, embedded files
- Office, RTF and legacy OLE files: macros, embedded objects, Equation Editor, remote template links
- ZIP-based files (docx, xlsx, jar, apk): unpacks and looks for executables, macros and path traversal
- Hidden payloads: trailing data, ZIP or EXE embedded in images, scripts inside images or SVGs, high entropy, packers
- Script rules for PowerShell, LOLBins, reverse shells, web shells and registry persistence
- SHA-256 and SHA-1, strings, and lookup links that use only the hash

**URLs** (one per line)
- Structure analysis: raw and obfuscated IPs, `user@host` tricks, homograph and punycode domains, shorteners, risky TLDs, free-hosting abuse, redirect parameters, risky download extensions
- Impersonation and misspelling checks against about 35 well-known brands
- Defanged output and link-outs to VirusTotal, urlscan.io, Safe Browsing and WHOIS
- URLs found inside scanned files are analyzed automatically

Each result gets a 0 to 100 score (0-14 low risk, 15-44 suspicious, 45+ high risk) with a reason list, and a JSON report can be copied or downloaded.

## Run locally

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

## Deploy to GitHub Pages

Pick one option.

**Option A: GitHub Actions (included)**
1. Push this repo to GitHub with the default branch named `main`.
2. Go to **Settings > Pages** and set **Source** to **GitHub Actions**.
3. Push any change (or run the workflow from the **Actions** tab). Your site appears at `https://<username>.github.io/<repo>/`.

**Option B: Deploy from a branch**
1. Go to **Settings > Pages**.
2. Set **Source** to **Deploy from a branch**, choose `main` and `/ (root)`, then save.

## Project layout

```
index.html                  the whole app
vendor/jszip.min.js         bundled ZIP reader (no CDN needed)
.github/workflows/pages.yml Pages deployment workflow
.nojekyll                   tells Pages to serve files as-is
```

## Limits

- No live reputation or domain-age lookups (these need a server); result cards link out instead.
- Only the first 16 MB of a large file are searched for strings and patterns.
- Signatures, brand lists and rules are in the `<script>` of `index.html` and are easy to extend.

## License

MIT. See `LICENSE` (replace `YOUR NAME` with your name). JSZip is bundled under its own license, see `THIRD_PARTY_NOTICES.md`.
