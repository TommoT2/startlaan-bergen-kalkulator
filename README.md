# Startlån & tilskudd i Bergen — interaktiv kalkulator

An interactive, single-file web calculator that helps first-time homebuyers in
Bergen, Norway understand **startlån** (the Norwegian municipal start-up loan
scheme administered via Husbanken) and the related **etableringstilskudd**
(establishment grant), compared against a regular private-bank mortgage.

The app and all of its content are in **Norwegian**, since the target audience
is young, first-time Norwegian homebuyers with little prior experience with
loans or property purchases. This README (project documentation) is in
English.

**Live demo:** enable GitHub Pages on this repo (see below) and the link will
appear at `https://<your-username>.github.io/<repo-name>/`.

## What it does

A single page, no build step, no backend. Everything runs client-side:

- **Household & income** — enter income for one or two adults in whichever
  period is most natural (hourly / weekly / monthly / yearly), as gross or net
  pay. The page converts seamlessly between all four periods and between
  gross/net using 2026 Norwegian tax rules (trinnskatt, trygdeavgift,
  minstefradrag, personfradrag).
- **Housing & loan** — purchase price, equity, existing debt, loan type
  (private bank / ordinary startlån / startlån for first-time buyers /
  startlån + tilskudd), and repayment period.
- **Interest rate** — floating vs. fixed (3/5/10/20-year) Husbanken rates,
  with Bergen kommune's 0.25 pp municipal markup, plus a slider to stress-test
  any rate level. Includes a short note on Norges Bank's rate outlook.
- **Results** — income vs. minimum/maximum thresholds chart, a rate-sensitivity
  chart, a side-by-side monthly payment comparison across loan types, and a
  detailed comparison table.
- **Requirements checklist** — live pass/warning flags against Bergen
  kommune's actual income ceilings, equity requirements, debt-to-income ratio,
  and stress test, plus the standard list of documentation everyone needs.
- **Glossary** — plain-language explanations of brutto/netto, gjeldsgrad,
  belåningsgrad, stresstest, grunnbeløp (G), etc., for someone who has never
  done this before.

## Tech

Plain HTML/CSS/JS, no framework, one external dependency loaded from a CDN
(Chart.js 4.4.1). No build step, no `node_modules`, no server required — it
works by opening `index.html` directly or by serving it via GitHub Pages.

## ⚠️ Important: this is not official, and not financial advice

This is an **unofficial educational tool**, not affiliated with Husbanken or
Bergen kommune, and it is not financial or legal advice. All figures
(interest rates, income thresholds, tax rates, the grunnbeløp) are manually
maintained and were last fact-checked in **June 2026**. Always verify current
numbers directly with:

- [Husbanken — startlån](https://www.husbanken.no/person/startlaan/)
- [Husbanken — renter](https://www.husbanken.no/rente/)
- [Bergen kommune — Boligkontoret](https://www.bergen.kommune.no/innbyggerhjelpen/bolig-og-sosiale-tjenester/okonomisk-stotte-og-radgivning/boligfinansiering/startlan-og-tilskudd-for-a-kjope-eller-beholde-bolig)
- [NAV — grunnbeløpet](https://www.nav.no/grunnbelopet)
- [Skatteetaten — satser](https://www.skatteetaten.no/en/rates/)

The grant ("tilskudd") estimate in particular is a rough heuristic — Bergen
kommune allocates it through an individual, discretionary assessment, not a
formula. Treat it as a directional signal only.

## Updating the figures each year

Nearly every number that changes annually lives in one block near the top of
the `<script>` tag in `index.html`, so a yearly refresh is a five-minute job:

| Constant | What it is | Where to check it |
|---|---|---|
| `RENTER` | Husbanken's current floating/fixed rates + the 0.25 pp Bergen kommune markup | husbanken.no/rente |
| `PRIVATBANK` | Equity requirement (currently 10%) and debt-to-income cap (5×) from utlånsforskriften | regjeringen.no — utlånsforskriften |
| `HUSHOLDNINGER` | Bergen kommune's income ceilings and max purchase price per household type, expressed as multiples of G | Bergen kommune's "Retningslinjer for startlån" PDF |
| `TILSKUDD` | Min/max grant size in multiples of G | Same PDF as above |
| `TAX2026` | Trinnskatt brackets, trygdeavgift, minstefradrag, personfradrag | skatteetaten.no/rates |
| `state.G` (default value) | The current grunnbeløp | nav.no/grunnbelopet (updates every 1 May) |

## Deploying — create the private repo + GitHub Pages

These commands assume you already have an SSH key configured for your GitHub
account (as `TommoT2` does) and either the GitHub CLI (`gh`) or plain `git`.

### Option A — using GitHub CLI (fastest)

```bash
cd startlaan-bergen
gh repo create startlaan-bergen-kalkulator --private --source=. --remote=origin
git add -A
git commit -m "Initial commit: interactive startlån/tilskudd calculator for Bergen"
git push -u origin main
gh repo edit --enable-pages
gh api -X POST repos/:owner/startlaan-bergen-kalkulator/pages -f source.branch=main -f source.path=/
```

> Note: `gh repo edit --enable-pages` support varies by `gh` version. If it
> errors, just enable Pages from the web UI instead (see Option B, step 4).

### Option B — plain git + GitHub web UI

```bash
cd startlaan-bergen
git init
git add -A
git commit -m "Initial commit: interactive startlån/tilskudd calculator for Bergen"
git branch -M main
git remote add origin git@github.com:TommoT2/startlaan-bergen-kalkulator.git
git push -u origin main
```

Then:

1. Go to `github.com/new`, create a **private** repository named
   `startlaan-bergen-kalkulator` (don't initialize it with a README — you're
   pushing your own), and skip straight to the push commands above.
2. In the repo, open **Settings → Pages**.
3. Under "Build and deployment", set **Source: Deploy from a branch**, branch
   `main`, folder `/ (root)`.
4. Save. The page goes live in 1–2 minutes at
   `https://tommot2.github.io/startlaan-bergen-kalkulator/`.

### ⚠️ One nuance worth knowing

On free/Pro personal GitHub accounts, enabling **GitHub Pages on a private
repository still publishes the rendered page publicly** — anyone with the
link can view the calculator, even though they cannot see your source code,
commit history, or repo settings (full private-Pages access control requires
GitHub Enterprise). That matches what you asked for here — a private
repo *and* a page you can share — just flagging it so the public-page part
isn't a surprise later.

## License

MIT — see `LICENSE`.
