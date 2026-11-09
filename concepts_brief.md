# Personal Passions, Making & Projects Portfolio (`www.severin.jp`)

* **Author:** Severin Ferrand
* **Date:** 2026-11-09

## Overview

`www.severin.jp` is an evergreen, low-maintenance personal portfolio website hosted on GitHub Pages to present what the author makes, writes, collects, and works on—replacing a retired WordPress site without using a date-ordered blog feed or a freeform visual mind map. It brings together varied personal domains in one browsable hub: culinary passions and structured tasting logs (wholesale couverture chocolate making/patisserie and a Cheese Pokedex), business and startup involvement (Coaster & Kaigan Brewery craft beers and Zeteoh), hands-on hobbies and habits (DIY projects, tennis), archived essays migrated from Google Docs, and a concise career summary pointing out to `resume.severin.jp` and LinkedIn.

There is no external event deadline; the project is designed so the author can build and expand sections incrementally over time with an AI coding agent (`Antigravity` + `modern-web-guidance`), zero ongoing server or dependency maintenance, and clean boundaries between on-site static pages and living Google Workspace data (Google Sheets dashboards, Google Docs notes, and Google Photos albums).

## Analysis

### Reviewer Status

| Reviewer Lens | Status |
| :--- | :--- |
| `egm-design` (Architecture, Scale & Quota Economics) | completed |
| `egm-codebase-details` (Observability & Operational Debuggability) | completed |
| `egm-zoom-out` (Contrarian / YAGNI & Code Minimization) | completed |
| `egm-comprehension` (UX & Developer Ergonomics) | completed |
| `egm-implementation-readiness` (Migration & Rollout Safety) | completed |
| `egm-structural` (Long-Term Tech Debt & Maintainability) | completed |
| `egm-inventive-catalyst` (Contradiction Elimination & Inventive Patterns) | completed |

### Contradiction 1: Scannable Navigation vs. Cross-Domain Variety (Replacing Both Blogs and Mind Maps)

* **The Conflict:** A chronological blog buries ongoing passions, ventures, and tasting catalogs under publication dates, while a 2D visual mind map fails mobile usability, keyboard navigation, and clear visual hierarchy. However, rejecting both without defining a clear structure risks turning a multi-hobby site into a flat miscellaneous link wall—or tempting AI coding sessions to invent custom layouts and JavaScript state machines for every new hobby.
* **Suggested Resolution Patterns (*Segmentation*, *Inversion*, *Intermediary*):**
  1. **Thematic Bento Hub with Four Fixed Content Archetypes:** Organize `www.severin.jp` as a single scannable CSS Grid Bento homepage backed by at most four reusable templates:
     * *Structured Collections / Pokedexes* (Couverture Chocolate catalog, Cheese Pokedex).
     * *Ventures & Career Pointer* (compact evergreen cards for Coaster & Kaigan Brewery, Zeteoh, and a two-sentence career bio linking to `resume.severin.jp` and LinkedIn).
     * *Notebook & Recipes* (clean typography for archived WordPress essays and finalized chocolate/patisserie recipes).
     * *Making, Sports & Habits* (visual project cards for DIY builds, handmade bonbons/bars, tennis, and active habits—omitting empty "future project" placeholders until work starts).
  2. **Inverted Mind Map (Grid First, Cross-Links & Shared Tags Inside):** Present a predictable grid up front and capture cross-topic connections through shared facet tags (for example, `#fermentation` connecting Couverture Cacao, Cheese Pokedex, and Coaster & Kaigan Brewery) and bidirectional links between items (such as a bonbon recipe linking to the couverture chocolates it uses). Label outbound cards with explicit destination badges (`Sheets Dashboard ↗`, `Photos Album ↗`, `resume.severin.jp ↗`).
* **Secondary Trade-Offs:** Requires keeping the set of layout archetypes capped at four and maintaining consistent slug IDs and tag names documented in an in-repo `AGENTS.md` file.

### Contradiction 2: Single-Source Google Workspace Editing vs. Workbook Privacy, Link-Farm UX, and Media Limits

* **The Conflict:** Keeping active logs, notes, and photos in Google Sheets, Google Docs, and Google Photos avoids maintaining data in two places, but naive linking creates three concrete traps:
  1. **Google Sheets Column Leakage & Owner-Cookie Blindness:** Google Sheets enforces read access at the **entire workbook level**, not per tab or hidden column. Linking publicly to a Dashboard or Canvas tab inside the master 60-row couverture spreadsheet exposes `Price`, `First purchase date`, and `Last purchase date` (viewers can copy the sheet, unhide tabs, or query export URLs). Conversely, restricted permissions look fine to the signed-in owner while showing `401/403` login walls to visitors.
  2. **Immutable Essays vs. Living Working Docs:** Sending readers to Google Docs for *retired WordPress articles* forces slow mobile editor chrome and hides text from same-origin search or Built-in AI—with zero maintenance benefit because archived essays no longer change.
  3. **Google Photos Expiration, EXIF GPS Leaks, and GitHub Pages' 1 GB / No-LFS Limit:** Direct `<img>` hotlinks to Google Photos (`lh3.googleusercontent.com`) expire with `HTTP 403` within roughly an hour; shared Google Photos albums expose home kitchen/workshop **EXIF GPS coordinates** unless location sharing is turned off per album; and committing raw 5–10 MB phone photos hits GitHub Pages' **1 GB site limit** (Git LFS cannot be used because GitHub Pages serves literal `.pointer` text files instead of images).
* **Suggested Resolution Patterns (*Extraction*, *Mutable-vs-Archival Split*, *Two-Tier Media*):**
  1. **Two-Workbook Isolation (or Allowlisted Static Export) for Sheets:** Keep the master 60-row couverture Google Sheet strictly **Private**. Build and share the public Google Sheets Dashboard (or Looker Studio view) on a **separate sanitized workbook** populated via `=QUERY(IMPORTRANGE(...))` that imports only allowlisted public columns (`Maker`, `Product Name`, `商品名`, `Details`, `コメント`)—or optionally let the AI agent periodically render those 5 columns into an on-site static table/JSON snapshot while linking out to the sanitized dashboard for charts. Verify all outbound links in an unauthenticated session.
  2. **One-Time Migration for Retired WordPress Articles, Outbound Links for Living Docs:** Convert immutable retired WordPress articles from Google Docs **once** into native static HTML pages in the repo (marking the old Docs archived), aided by a custom `404.html` page to guide visitors arriving from old WordPress URLs. Keep actively edited working notes/recipes in Google Docs (`/pub` or view-only) if desired.
  3. **Local EXIF-Stripped WebP Thumbnails + Hardened Outbound Photo Albums:** Commit only small, EXIF-stripped `.webp`/`.avif` cover images (`30–120 KB` each, `< 1.5 MB` per page, zero Git LFS) in Git, and link out to Google Photos Shared Albums for large DIY/making photo sets with *"Share photo location" = OFF* and *"Collaboration / Comments" = OFF*.
* **Secondary Trade-Offs:** Requires one-time setup of the secondary sanitized Google Sheet (matching columns carefully if the master sheet layout changes) and stripping EXIF metadata when adding cover thumbnails.

### Contradiction 3: Multi-Year Zero-Dependency Longevity vs. Multi-Page Copy-Paste Drift

* **The Conflict:** On a personal site updated occasionally over several years, npm build pipelines and frontend frameworks accumulate broken dependencies and Node version mismatches during idle periods. At the opposite extreme, copy-pasting `<head>`, header, and footer HTML across dozens of standalone essay and hobby files creates markup drift and inflates AI coding token usage whenever navigation changes.
* **Suggested Resolution Patterns (*Zero-Build Native Platform*, *Extracted Shell*):**
  1. **Zero-NPM-Build Static Files on GitHub Pages:** Use zero `package.json` bundlers and zero frontend frameworks. Rely on semantic HTML, Baseline modern CSS (CSS Grid, Custom Properties, `light-dark()`, multi-page `@view-transition { navigation: auto; }`, native `<details>` and `<dialog>`), clean directory URLs (`/chocolate/`, `/writing/slug/`), `.nojekyll`, and `lang="ja"` attributes on Japanese text (`商品名`, `コメント`).
  2. **Shared Chrome via Lightweight Web Components + `AGENTS.md` Blueprint:** Centralize shared header/footer chrome in a tiny zero-dependency custom element (`<site-shell>` with reserved CSS height to avoid layout shift) and document canonical card/page HTML patterns in `AGENTS.md` so future AI sessions stay consistent over time.
  3. **Safe DNS Cutover & Zero-Maintenance Monitoring:** Stage first on the GitHub Pages subpath using relative asset paths before committing root `CNAME` (`www.severin.jp`), keep `resume.severin.jp` on its own isolated DNS record, and enable DNS-verified Google Search Console for zero-cookie email alerts on TLS issues or legacy WordPress 404 spikes.
* **Secondary Trade-Offs:** Global `<head>` metadata edits across static article files require a quick batch pass by the AI agent or a standard-library script rather than a server template.

### Contradiction 4: Optional Chrome Built-in AI Experiments vs. Zero Maintenance & Cross-Browser Reach

* **The Conflict:** As a Google Chrome team member, experimenting with Chrome Built-in AI Web APIs (`Translator`, `Summarizer`, `LanguageModel`) fits naturally with bilingual EN/JP tasting notes and long-form essays—but mobile browsers, Safari, and Firefox do not support desktop Gemini Nano (`>= 22 GB` free disk gate), and Origin-Trial-gated APIs expire every 6–12 weeks, creating recurring maintenance toil.
* **Suggested Resolution Patterns (*Isolated Progressive Enhancement*):**
  1. **Token-Free Stable APIs Only & Default-Hidden Controls:** Restrict persistent production enhancements to shipped Stable Chrome APIs that require **zero Origin Trial tokens** (such as inline EN↔JP note translation via `Translator` or optional 3-bullet article summaries via `Summarizer` on migrated on-site essays). Isolate all detection (`'Translator' in self`, `await *.availability()`) inside one optional module (`ai-enhancements.js`), keep controls `hidden` unless supported, gate `'downloadable'` states behind explicit user clicks with download progress feedback, or house experimental Prompt API prototypes inside a dedicated "Chrome Web AI Lab" hobby card.
* **Secondary Trade-Offs:** Mobile and non-Chrome visitors simply read the complete static HTML content without seeing desktop-only AI buttons.

### Ideal Final Result

Whenever the author finishes a DIY build, updates a chocolate/cheese log, or publishes an essay months from now, updating `www.severin.jp` takes either adding rows to their private Google Sheet (projected safely through a sanitized public sheet with zero private price leakage) or asking an AI agent to add a card/article following `AGENTS.md`—with zero `node_modules` to break, zero server bills, permanent URLs on GitHub Pages, safe coexistence with `resume.severin.jp`, and clean progressive Chrome Built-in AI touches for desktop Chrome visitors.

## Goals vs. Non-Goals

* **In-Scope Goals:**
  * Design `www.severin.jp` as an evergreen, non-chronological Bento-style personal hub on GitHub Pages covering culinary collections (Couverture Chocolate, Cheese Pokedex), ventures (Coaster & Kaigan Brewery, Zeteoh), hobbies/DIY (tennis, making, habits), migrated WordPress essays/recipes, and outbound links to `resume.severin.jp` and LinkedIn.
  * Enforce a strict privacy and maintenance boundary across Google Workspace assets: two-workbook column isolation for Google Sheets dashboards (protecting `Price` and purchase dates), one-time static HTML migration for immutable archived WordPress docs, and local EXIF-stripped `.webp` thumbnails paired with location-scrubbed Google Photos album links.
  * Keep the codebase zero-build (plain HTML, modern CSS, vanilla ES modules / Web Components, `.nojekyll`, and an `AGENTS.md` structural guide) so it stays editable after long gaps between updates.
  * Support optional, zero-token-maintenance Chrome Built-in AI progressive enhancements (`Translator`, `Summarizer`) isolated from core static rendering.
* **Out-of-Scope Non-Goals:**
  * Rushing a 3-minute live presentation or physical tasting companion tool for a team work event (rolled back and canceled).
  * Hosting a formal CV/resume directly on `www.severin.jp` (delegated to `resume.severin.jp` and LinkedIn).
  * Running backend servers, databases, npm/Node bundler pipelines, Git LFS, or paid cloud LLM API keys.
  * Publishing empty "coming soon" or "future project" stub pages before content exists.

## Alternatives Weighed

* **Standard chronological Jekyll blog layout:** Rejected by author because a date-ordered blog feed is dull to read through and buries evergreen passions, structured collections, and ventures under timestamps.
* **Visual 2D mind-map navigation interface:** Rejected by author due to poor usability, weak mobile ergonomics, and unclear hierarchy.
* **Using `severin.jp` for a rushed 3-minute team chocolate presentation:** Rolled back and abandoned by author in favor of keeping the concept focused on an evergreen multi-hobby personal website to build later without artificial event constraints.
* **Sharing a Dashboard tab directly inside the private master Google Sheet:** Rejected during technical stress-testing because Google Sheets permissions apply to the whole file, exposing hidden `Price` and purchase-date columns to viewers unless projected into a separate sanitized workbook (or Looker Studio / static export).
* **Frontend framework / npm static-site-generator build pipeline:** Rejected during technical stress-testing because `node_modules` and bundler toolchains decay over multi-month gaps between personal site updates.
* **Direct `<img>` hotlinking to Google Photos or using Git LFS on GitHub Pages:** Rejected during technical stress-testing because Google Photos image URLs expire (`HTTP 403`) and GitHub Pages serves raw LFS pointer text files instead of binary images.

## Key Decisions & Staging for Next Steps

* **Locked Concept Foundations (Saved for Later Implementation):**
  * **Information Architecture:** Responsive CSS Grid Bento hub organized into four fixed archetypes (*Structured Collections*, *Ventures & Career*, *Notebook & Recipes*, *Making & Habits*), connected by shared topic tags and item cross-links rather than dates or a 2D graph.
  * **Hosting & Stack:** GitHub Pages (`www.severin.jp` + apex `severin.jp`, keeping `resume.severin.jp` isolated) serving zero-build semantic HTML, modern CSS, lightweight `<site-shell>` Web Components, `.nojekyll`, and an `AGENTS.md` blueprint.
  * **Workspace & Media Boundaries:** Separate sanitized Google Sheet (via `QUERY`/`IMPORTRANGE` excluding `Price` and dates) for outbound Sheets Dashboards; one-time static HTML import for retired WordPress Google Docs; EXIF-stripped local `.webp` thumbnails (`30–120 KB`) plus hardened Google Photos Shared Albums for bulk galleries.
  * **Chrome Built-in AI:** Strictly optional, default-hidden progressive enhancement using Stable token-free APIs (`Translator`, `Summarizer`) inside an isolated module.
* **Suggested Incremental Build Order When Resuming Later:**
  1. **Milestone 1 (Core Hub & Staging):** Scaffold root `index.html` Bento hub, shared CSS tokens, `<site-shell>`, `.nojekyll`, custom `404.html`, and `AGENTS.md`; verify on GitHub Pages staging subpath before DNS/`CNAME` cutover.
  2. **Milestone 2 (First Deep Sections):** Set up the sanitized public Couverture Google Sheet Dashboard + `/chocolate/` maker/recipe page, venture cards (Coaster & Kaigan Brewery, Zeteoh, Career links), and initial Cheese Pokedex schema.
  3. **Milestone 3 (Archive Migration & Optional Chrome AI):** Batch-convert selected evergreen WordPress articles from Google Docs into `/writing/`, then layer optional `ai-enhancements.js` (`Summarizer` / `Translator`) on top.
