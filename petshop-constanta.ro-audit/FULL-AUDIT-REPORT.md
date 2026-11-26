# Audit SEO complet — petshop-constanta.ro

**Data:** 26 noiembrie 2026 · **Metodă:** crawl live al tuturor URL-urilor din sitemap (509 pagini HTML), Lighthouse 12 local (mobil, throttling simulat), analiza codului temei din repo · **Tip business detectat:** e-commerce + magazin local fizic (PetStore, Constanța)

## Scor de sănătate SEO: 74 / 100

| Categorie | Pondere | Scor |
|---|---|---|
| SEO tehnic | 22% | 90 |
| Calitatea conținutului | 23% | 60 |
| On-page (titluri, meta, headings) | 20% | 70 |
| Schema / date structurate | 10% | 85 |
| Performanță (CWV, laborator) | 10% | 70 |
| Pregătire pentru căutare AI | 10% | 60 |
| Imagini | 5% | 95 |

Fundația tehnică e solidă: toate cele 509 pagini răspund 200, fără redirect-uri interne, canonicale corecte, `index, follow` peste tot, schema Product completă pe toate produsele, Lighthouse SEO 100. Punctele slabe sunt **titlurile duplicate pe produse**, **conținutul subțire pe colecții și blog** și **LCP-ul paginii principale**.

### Top 5 probleme

1. **86 de pagini de produs împart 30 de titluri duplicate**, multe trunchiate în mijlocul frazei (ex. 13 conserve Luger's au toate titlul „Luger's Hrană Umedă Conservă Câini cu – Petshop Constanța”). Titlul vine din câmpul „Search engine listing” al fiecărui produs din Shopify Admin, nu din temă.
2. **Tema publicată pe site diferă de cea din repo.** Live, imaginea hero are un `srcset` defect (`550w, <original fără descriptor>`) și e preîncărcată de două ori, de pe două domenii (`www.petshop-constanta.ro/cdn` și `cdn.shopify.com`). Codul din repo (`theme/sections/hero-banner.liquid`) are deja varianta corectă (550/750/950/1200w + un singur preload), dar nu e publicat.
3. **LCP pe pagina principală 4,6 s (mobil, laborator)** — scor performanță 71. Cauza principală e imaginea hero de mai sus (45% din LCP e „load delay”). Colecțiile sunt rapide (96, LCP 1,9 s), produsele acceptabile (80, LCP 3,9 s).
4. **Blogul e practic gol**: un singur articol; `/blogs/ghiduri` are 8 cuvinte; ambele bloguri au același titlu („Blog Îngrijire Animale – Petshop Constanța”).
5. **17 colecții au sub 120 de cuvinte** de conținut propriu (ex. hrană pești, accesorii pești, suplimente rozătoare/păsări) — prea puțin context pentru a se clasa pe interogările de categorie.

### Top 5 câștiguri rapide

1. Publicați tema din repo (rezolvă `srcset`-ul hero și preload-ul dublu, fără cod nou).
2. Rescrieți cele 30 de titluri duplicate incluzând diferențiatorul (aromă, gramaj, culoare/mărime) — lista completă e în `findings/liste-detaliate.md`.
3. Scurtați cele 121 de titluri peste 60 de caractere, păstrând „– Petshop Constanța” doar unde încape.
4. Adăugați GTIN/EAN pe cele 344 de produse care nu îl au (Merchant Center + rich results) și SKU pe cele 45 lipsă.
5. Corectați descrierea produsului `lugers-hrana-umeda-conserva-caini-cu-vita-cartof-400g`, unde subtitlurile sunt formatate ca H1 (7 H1 pe pagină).

---

## 1. SEO tehnic — 90

**Ce funcționează**
- HTTPS cu HSTS, redirect canonic `petshop-constanta.ro` → `www.` (301), fără lanțuri.
- `robots.txt` (implicit Shopify) permite conținutul public și blochează coș/checkout/cont și capcanele de filtre/sortare.
- Sitemap index cu 6 sub-sitemap-uri, 510 URL-uri, toate 200.
- Canonical prezent pe toate paginile. Cele 6 „nepotriviri” găsite sunt doar diferențe de majuscule în codificarea procentuală (`%C8%99` vs `%c8%99`), echivalente conform RFC 3986 — nu e o problemă.
- `lang="ro"` pe toate paginile; fără hreflang (corect, site monolingv).
- TTFB median 174 ms în crawl; pagini HTML mediane de 216 KB.

**Probleme**
- **Ridicat — tema live ≠ repo.** Optimizările de performanță din branch-ul `claude/petshop-constanta-mobile-perf-tv3grl` nu sunt pe site. Până la publicare, orice audit al codului din repo nu reflectă ce vede Google.

## 2. Calitatea conținutului — 60

- Produse: median 208 cuvinte (minim 110) — acceptabil pentru e-commerce; descrieri cu compoziție, constituenți analitici și ghid de hrănire, ceea ce e un plus de E-E-A-T.
- **Mediu — 17 colecții sub 120 de cuvinte** (minim 58). Adăugați 100–200 de cuvinte de introducere utilă: cui se adresează categoria, cum alegi, livrarea în Constanța.
- **Ridicat — blog**: 1 articol („Îngrijirea pisicilor vara”, 370 de cuvinte). Pentru un petshop local, ghidurile (hrănire pe vârste, sterilizare, acvaristică pentru începători) sunt principala sursă de trafic informațional și de citări în răspunsurile AI.
- **Scăzut — 25 de pagini cu meta descrieri duplicate** (10 grupuri), aproape toate variante de produs (culcușuri Taurus, Versele-Laga etc.).
- Pagini legale scurte (`/pages/politica-cookies`, `/pages/anpc` ~75 de cuvinte) — normal, fără impact.

## 3. On-page — 70

- Titluri: toate prezente; mediană 58 de caractere; **121 peste 60** (vor fi trunchiate în SERP); 15 sub 30.
- **Ridicat — 30 de grupuri de titluri duplicate / 86 de pagini.** Cauza: variantele aceluiași produs sunt produse separate, iar SEO title-ul a fost scris (sau trunchiat automat) fără diferențiator. Cele mai mari grupuri: Luger's câini (13), culcuș textil Taurus (11), Tropical Supervit (4), Luger's pisici (4).
- Meta descrieri: prezente pe 100% din pagini, toate între 70 și 160 de caractere.
- H1: exact unul pe 508/509 pagini; excepția e produsul Luger's vită & cartof (7 H1).
- Link-uri interne: median 50 de link-uri unice per pagină — navigare bună.

**Observație de arhitectură:** 11 culcușuri Taurus și 13 conserve Luger's ca produse separate concurează între ele pe aceleași interogări. Pe termen lung, gruparea lor ca **variante ale unui singur produs** (culoare/mărime, aromă) consolidează semnalele și elimină duplicarea.

## 4. Schema / date structurate — 85

- 0 erori de parsare JSON-LD pe 509 pagini.
- **Produse (437/437):** Product, Offer cu `availability`, `MerchantReturnPolicy`, `OfferShippingDetails` cu timp de livrare, Brand, BreadcrumbList, imagine, descriere.
- **Homepage:** PetStore complet (adresă, geo, telefon, program, `hasMap`, plăți acceptate), Organization, WebSite + SearchAction, FAQPage, OfferCatalog.
- **Colecții:** CollectionPage + ItemList (37/53) + BreadcrumbList (53/53).
- **Mediu — GTIN lipsă pe 344/437 produse** și SKU lipsă pe 45. GTIN-ul ajută la potrivirea în Google Shopping și la rich results.
- **Scăzut — fără `aggregateRating`/`review`.** Corect cât timp nu există recenzii reale; când adăugați o aplicație de recenzii, marcați-le.
- **Scăzut — 16 colecții fără ItemList/CollectionPage** (53 au Breadcrumb, doar 37 au ItemList).
- Descrierea din schema Product conține entități HTML (`&amp;`) și e foarte lungă; se poate scurta la primele 2–3 fraze.

## 5. Performanță — 70 (Lighthouse mobil, laborator)

| Pagină | Perf | LCP | TBT | CLS | SEO | A11y |
|---|---|---|---|---|---|---|
| Homepage | 71 | 4,6 s | 450 ms | 0 | 100 | 96 |
| Produs (Golosi 15 kg) | 80 | 3,9 s | 320 ms | 0 | 100 | 93 |
| Colecție `/collections/caini` | 96 | 1,9 s | 200 ms | 0 | 100 | 95 |

- **Ridicat — hero homepage.** Defalcare LCP: TTFB 13%, *load delay* 45%, *load time* 37%, render 5%. `srcset`-ul live are doar `550w` + originalul fără descriptor, iar imaginea e preîncărcată de două ori de pe două origini. Fișierul în sine e mic (WebP 12–20 KB servit de CDN), deci problema e descoperirea/prioritizarea, nu greutatea.
- **Mediu — JavaScript pe firul principal:** 4,0 s main-thread pe homepage; cel mai mare consumator e documentul însuși (1,8 s), apoi pixelii Shopify/web-pixels (0,6 s). Merită revizuite aplicațiile și pixelii instalați (Facebook, Pinterest, GTM, nfcube).
- CLS 0 peste tot — excelent.
- Datele de teren (CrUX) nu au putut fi obținute: cota anonimă a API-ului PageSpeed era epuizată.

## 6. Pregătire pentru căutare AI — 60

- `llms.txt` există, dar e textul implicit Shopify (instrucțiuni pentru agenți de cumpărături), nu un rezumat al magazinului. Shopify îl generează automat; nu e o prioritate.
- Punct forte: schema PetStore + FAQPage pe homepage, date clare despre livrare (aceeași zi în Constanța până la 14:00) — exact tipul de fapte pe care asistenții AI le citează.
- Punct slab: lipsa conținutului informațional (blog) — modelele AI citează ghiduri, nu pagini de categorie.

## 7. Imagini — 95

- 6.846 de imagini; doar 7 fără `alt` (pe 6 pagini) și 5 fără dimensiuni.
- Lighthouse semnalează pe homepage `alt` redundant cu textul vizibil — minor.

---

## Limitări

- Lighthouse a rulat local din cloud (throttling simulat), nu din PageSpeed Insights; valorile sunt orientative, nu date de teren.
- Fără acces la Google Search Console / GA4 (indexare reală, interogări, trafic) și fără date de backlink.
- `cdn.shopify.com` nu a fost accesibil din mediul de audit, deci greutatea celui de-al doilea preload nu a fost măsurată direct.

Liste complete (titluri și descrieri duplicate, colecții subțiri, produse fără SKU): `findings/liste-detaliate.md`. Plan de acțiune pe faze: `ACTION-PLAN.md`.
