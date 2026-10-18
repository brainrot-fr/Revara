# Project context (from the design-system chat)

Last updated: 2026-10-18. Use this file to brief a new chat or a coding agent. Brand name is a placeholder: **{Brand}** ("Revara" is already registered and was dropped).

## 1. Product
India-only web store for **repairable, wireless-first, customizable mechanical keyboards**. Users configure a keyboard (65%, 75%, 100%), see a live **2D** preview, buy with Razorpay (INR), track the order. A small team runs the shop from a custom admin area. Inspired by Framework (repairability, customizability). Repo: https://github.com/brainrot-fr/Revara/ (I could not open it; everything came from pasted READMEs: root, `Design/UI`, `Design/MASTER.md` (empty), `WebApp/README.md`). README mentions 62 Mt of e-waste in 2022 (WHO fact sheet) as the motivation.

## 2. Stack and constraints
React + Vite + TypeScript · **Tailwind CSS v4 + shadcn/ui set up from scratch** (no `components.json` yet) · Supabase (auth: Google + email, Postgres, storage) · Vercel · Razorpay · `lucide-react` icons · **dark mode in v1** (class-based, Light/Dark/System).
**Rule:** only shadcn/ui components; no other library; small app-specific components allowed in `src/components/app/` built from shadcn parts.
Tooling plan: **Rivermark.ai** generates UI from `index.html` + prompts; a **coding agent** then implements in the repo. (User once typed "riverside.ai"; assumed to mean Rivermark.ai.)

## 3. Hardware / firmware
Custom matrix PCB (a ZMK *shield*) with a **nice!nano-style controller** (a ZMK *board*), firmware **ZMK**, so **wireless (BLE) is the default**; wired USB stays an option; battery is part of the default build. Each of 65/75/100 needs its own shield definition and physical layout. Keymap options discussed: default keymap (always), **ZMK Studio** (live editing, needs Studio-enabled build + unlock key + physical-layout metadata), GitHub config template (advanced; fork, edit `.keymap`, Actions builds `.uf2`, flash by double-tap reset). **Decision pending** on which is officially supported (recommended: default + Studio, GitHub as advanced guide).

## 4. Visual system (see `DESIGN.md`, `index.html`)
Direction "Calm workshop": lavender-white + muted violet. Tokens (light): background `#FAF8FD`, foreground `#2A2233`, card `#FFFFFF`, primary `#6B4FBB` (hover `#5B41A5`), secondary `#EFE9FA`, muted `#F1EDF8`, muted-fg `#675E75`, accent `#EDE6F9`, border `#E4DDF0`, input `#D6CCE6`, ring `#8466D1`, destructive `#B3263E`, success `#2F7D5B`, warning `#8A5A00`, info `#3A62B8`, sakura `#F4C9DA` (decorative only). Dark tokens defined (`#17131F` background, primary `#A58BF0`).
Fonts: **Plus Jakarta Sans** (headings, prices) + **Manrope** (body) + JetBrains Mono (IDs). 4px grid, radius 0.75rem, soft violet shadows, control heights 32/40/48, icons Lucide 16/20/24. WCAG 2.2 AA, visible focus, reduced-motion respected.
Patterns documented: nav, forms, buttons, cards, option cards, dialogs, tables, empty/loading/error, reviews (stars), discount code, claims badges, admin density, 2D `KeyboardPreview` (layered SVG, layout JSON, CSS-variable colourways).
Starter colourways (product data in Supabase, not theme): Lilac `#B9A6E8`, Sakura `#F4C9DA`, Graphite `#3B3744`, Ivory `#F3EEE3`.

## 5. Brand / logo
Logo concept: **keycap, switch and socket floating apart**, low three-quarter view (user's reference photo: teal keycap on clear switch on 3D-printed blue base). User's AI attempt was a soft 3D render with "legs" and was rejected for logo use. I hand-built a flat SVG (`brand/logo-mark.svg`, `brand/logo-mark-mono.svg`; generator script was `/home/claude/gen.py`, not saved to outputs). Interim only; hire a designer later. Wordmark pending the name; set in Plus Jakarta Sans Bold.
Name ideas given (unchecked): Reseat, Tenon, Mendkey, Kintsu, Dovetail. Must do trademark search (IP India, WIPO) before buying a domain; `revara.org` idea was dropped.

## 6. Pages and decisions (see `ADR-001`, revision 3)
**37 routes:** 27 customer + 10 admin. Configurator-first (presets open the configurator; no separate product page); single-page accordion configurator beside sticky preview (order: Layout, Case, Sockets, Switches, Keycaps, Controller, Battery, PCB, Review).
Customer: Home, Keyboards, Configurator (`/customize…`), Repairability, Firmware, Cart (+sheet), Checkout, Auth (+forgot/reset/callback), Orders, Order detail (tracking, review dialog, warranty line), Saved designs, Account, About, Support, **Claim form**, policies (shipping, returns, warranty, privacy, terms), 404/error.
Admin: Dashboard, Orders, Order detail, Build queue, Catalog (parts incl. warranty months, colourways, presets, layouts), Discounts, Reviews, **Claims**, Customers, Settings.
Decided: no guest checkout; India only, INR; flat shipping rate (admin setting); switches/keycaps choose-or-bring-your-own; saved designs need an account; reviews (verified buyers, per layout, text only, auto-publish) and one discount code per order are in v1; **GST invoices emailed manually** (admin tracks "invoice emailed").
**Returns/warranty:** no returns or exchanges of builds; refund only for an approved claim when the product is returned **at the buyer's expense** to a specified address; **free replacement parts** with genuine proof; **1-year warranty** on products the admin selects; checkout has a required acknowledgement checkbox.

## 7. Files delivered (all in `/mnt/user-data/outputs`)
`DESIGN.md` · `index.html` (token/component harness with working 2D preview demo, theme toggle, brand mark) · `brand/logo-mark.svg`, `brand/logo-mark-mono.svg` · `ADR-001-pages-and-information-architecture.md` · `RIVERMARK-PROMPTS.md` (master prompt + P00-P29, P99 dark pass, consistency fixer) · `POLICY-TEMPLATES.md` (shipping, returns, warranty, privacy, terms, grievance officer; placeholders `[[...]]`) · `context.md`.

## 8. Not verified / caveats
- Repo never inspected; no theme or config files were changed.
- Harness and logo were checked with a PIL render, not a browser; prompts were not run in Rivermark.ai.
- Policy templates are **not legal advice**; lawyer review needed. Indian e-commerce rules expect return/refund/warranty info and a visible grievance officer (acknowledge in 48 h, resolve in a month); verify current text. Razorpay live-account requirements to be checked.
- Part names, prices, battery sizes in prompts are sample content.
- Keymap-editor GUI tools were mentioned from memory only.

## 9. Open items
Brand name + trademark/domain · final logo/hero art · supported keymap path and `/firmware` copy · shield + layout JSON for 65/75/100 · refund-eligible reasons, claim window, who pays for returned faulty parts · which products carry the warranty · legal entity, GSTIN, return address, grievance officer · lawyer review · real colourway list per part.

## 10. Working preferences
User wants: practical, specific outputs; asks for options and recommendations; decides quickly; prefers files for deliverables (markdown + HTML), explanations in plain language; no questions unless needed (state assumptions instead).

## 11. Suggested next steps
1. Fill policy placeholders and get review. 2. Paste `index.html` and the master prompt into Rivermark.ai; run P00, P01, P02… in order; run P99 per approved page. 3. Define layout JSON, parts, prices, compatibility rules. 4. Run `npx shadcn@latest init`, paste tokens from `DESIGN.md` §8. 5. Pick the name and replace `{Brand}`.
