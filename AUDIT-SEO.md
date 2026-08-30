# Audit SEO — petshop-constanta.ro

**Data:** 30 august 2026
**Context:** analiza live a site-ului (`petshop-constanta.ro`) și a instrumentelor
externe (PageSpeed, Rich Results Test etc.) nu au putut fi accesate din acest
mediu de lucru — accesul de rețea e blocat de politica sesiunii (`EGRESS_BLOCKED`).
Auditul de mai jos e făcut **static, pe codul temei din acest repo**
(`theme/layout/theme.liquid`, `theme/sections/header.liquid`,
`theme/sections/hero-banner.liquid`), folosind metodologia și referințele
pluginului `claude-seo` instalat în `.claude/plugins/claude-seo/`.

**Important:** aceste 3 fișiere nu sunt toată tema. Snippet-urile `meta-tags`,
`analytics-head`, `gdpr-consent`, `cart-drawer`, footer-ul, templateurile de
produs/colecție/blog, `robots.txt` și `sitemap.xml` **nu sunt mirror-uite în
acest repo** — nu pot fi auditate de aici. Vezi secțiunea „Ce n-am putut
verifica" mai jos pentru lista completă.

---

## Ce e deja bine

- **JSON-LD dedublat corect** — schema Organization era randată o singură
  dată, sitewide, în `<head>`, nu repetată pe fiecare secțiune.
- **Return policy la nivel de Organization** — folosirea `hasMerchantReturnPolicy`
  direct pe Organization (nu doar per-produs) e exact pattern-ul introdus de
  Google în noiembrie 2025 (configurare politică retur/livrare fără cont
  Merchant Center, direct din Search Console).
- **Navigare 100% server-rendered** — meniul desktop (mega menu), drawer-ul
  mobil și toate linkurile de categorii sunt `<a href>` reale randate de
  Liquid din `linklists`, nu construite din JS. Googlebot vede toată
  structura de categorii fără să execute JavaScript.
- **Aria-labels și semantică corectă** — `<nav aria-label="Navigare
  principală">`, search cu `role="search"` + `aria-label`, butoane cu
  `aria-label` pe iconițe fără text — bune și pentru accesibilitate, și ca
  semnal indirect de calitate a paginii.
- **Un singur H1 clar pe homepage** — titlul din `hero-banner.liquid`
  (`section.settings.title`) e singurul H1, cu text descriptiv, nu generic
  gen „Bine ați venit".
- **Alt-text corect diferențiat** — logo-ul are `alt="{{ shop.name }}"` (text
  real, ca la un logo), iar imaginea de fundal a hero-ului are `alt=""` +
  `role="presentation"` (corect: e strict decorativă, mesajul e deja în H1/
  subtitle ca text real, nu ca text-în-imagine).
- **`lang="ro"` setat pe `<html>`** — corect pentru un magazin single-market.

---

## Probleme găsite și corectate (în acest repo, `theme/layout/theme.liquid`)

### 1. Proprietate greșită în schema de retur: `applicableCountry` în loc de `returnPolicyCountry`

Din martie 2025, Google cere explicit proprietatea **`returnPolicyCountry`**
în `MerchantReturnPolicy` (nu `applicableCountry`, care e proprietatea
generică Schema.org, dar nu cea validată de Google pentru rich results/
Merchant Listings). Codul avea `applicableCountry` — politica de retur risca
să nu fie recunoscută de Google chiar dacă JSON-ul era altfel valid.
**Fix:** redenumit în `returnPolicyCountry`.

### 2. Logo lipsă din schema Organization dacă `settings.logo` nu e setat

Proprietatea `"logo"` era inclusă condiționat — dacă tema nu are un logo
setat în `settings.logo` (setarea globală a temei), schema Organization
rămânea complet fără `logo`, deși `header.liquid` are deja un fallback la
sigla din `assets/petshop-constanta-sigla.avif` pentru afișare. Un logo
lipsă în schema Organization slăbește potențialul de knowledge panel/entity
recognition în Google. **Fix:** aceeași siglă de fallback e folosită acum și
în JSON-LD, deci schema are mereu un logo.

### 3. Lipsea `@id` (entity linking) și schema `WebSite`

Fără `@id`, Organization e o entitate „plutitoare" în graf — Google nu are
un ancoraj stabil de legat de alte scheme (WebSite, BreadcrumbList,
Product.brand etc.) de pe restul site-ului. **Fix:** Organization și
WebSite sunt acum grupate într-un singur `@graph`, cu `@id` stabile
(`{shop.url}/#organization`, `{shop.url}/#website`), iar WebSite referă
Organization prin `publisher`.

---

## Recomandări care necesită date reale de la tine (nu le-am inventat în cod)

### A. Schema `PetStore` / `LocalBusiness` — cel mai important gap

Site-ul are pagini pentru magazine fizice (`/pages/magazine` apare în
footer-ul drawer-ului mobil), deci e o afacere locală cu prezență fizică —
dar singura schema sitewide e `Organization` generică. Tipul corect
Schema.org pentru un magazin de animale este **`PetStore`** (subtip
`LocalBusiness`, suportat oficial de Google pentru rich results locale).

Fără el, magazinul nu are avantajul de map pack / knowledge panel local pe
care `Organization` simplă nu îl oferă (adresă, telefon, orar, geo). Pentru
a-l adăuga corect am nevoie de la tine (per magazin, dacă sunt mai multe):

- adresă completă + oraș + cod poștal
- telefon
- orar de funcționare (openingHours)
- coordonate geo (opțional, dar recomandat)
- linkuri social media (`sameAs`) pentru entity disambiguation

Nu am completat aceste date cu valori generice/placeholder în cod — un
`PetStore` cu date fictive sau incomplete poate fi respins de Google Rich
Results Test sau, mai rău, poate crea inconsistențe NAP (name/address/
phone) dacă nu se potrivește exact cu Google Business Profile.

**Recomandare:** odată ce îmi dai datele magazinului/magazinelor, adaug un
bloc `PetStore` (sau mai multe, dacă sunt mai multe locații) în același
`@graph` din `theme.liquid`, legat de Organization prin `@id`.

### B. `sameAs` (Facebook, Instagram etc.) pe Organization

Ajută Google să lege entitatea site-ului de profilurile sociale reale (semnal
de E-E-A-T / trust). Trimite-mi linkurile paginilor oficiale și le adaug.

---

## Ce n-am putut verifica din acest mediu (necesită acces live sau mai mult cod în repo)

- **`robots.txt` / `sitemap.xml`** — generate automat de Shopify, dar
  merită verificat dacă nu exclud accidental pagini importante.
- **Snippet-ul `meta-tags`** (`{% render 'meta-tags' %}` din `theme.liquid`)
  — aici stau probabil `<link rel="canonical">`, `meta description`,
  `og:*`/`twitter:*`. Nu e în acest repo, deci nu pot confirma dacă
  canonicalul e corect setat pe fiecare tip de pagină (produs, colecție,
  filtrare/paginare).
- **Template-uri de produs și colecție** — Product schema (`price`,
  `availability`, `aggregateRating`, `sku`, `gtin`), `BreadcrumbList`, title
  tags/meta descriptions per produs. Nu sunt în acest repo.
- **Core Web Vitals live (LCP/INP/CLS pe date reale)** — vezi
  `AUDIT-PERFORMANTA-MOBIL.md` pentru auditul de performanță deja făcut
  static; datele de teren (CrUX) necesită acces live sau `/seo google` cu
  o cheie PageSpeed API.
- **Pagina `/pages/magazine`** — dacă listează adresele fizice, ar trebui
  să aibă propriul `PetStore`/`LocalBusiness` per locație (vezi punctul A).

---

## Pași următori recomandați

1. **Trimite-mi datele de magazin** (adresă, telefon, orar, social media) ca
   să completez schema `PetStore` — cel mai mare câștig SEO local disponibil.
2. **Deblochează accesul de rețea la `petshop-constanta.ro`** pentru acest
   mediu (sau rulează `/seo audit` local, pe calculatorul tău) ca să pot
   confirma canonical/meta description/robots.txt/sitemap și rula un audit
   tehnic complet, live.
3. **Aplică fix-urile din `theme.liquid`** în tema live (Admin → Edit code,
   sau prin API la confirmarea ta) — nu au fost urcate automat.
4. După aplicare, validează cu
   [Google Rich Results Test](https://search.google.com/test/rich-results)
   pe homepage, ca să confirmi că `returnPolicyCountry` e recunoscut.
