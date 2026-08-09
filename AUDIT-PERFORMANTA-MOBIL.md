# Audit performanță mobil — petshop-constanta.ro

**Data:** 9 august 2026
**Context:** analiză pornită de la raportul PageSpeed Insights (mobil).
Raportul PSI în sine nu a putut fi deschis din acest mediu de lucru (accesul
la `pagespeed.web.dev` și la site-ul public este blocat de politica de rețea
a sesiunii), așa că auditul s-a făcut direct pe **codul temei live**, citit
prin Shopify Admin API, plus dimensiunile reale ale fișierelor media din
magazin.

---

## Ce este deja bine în temă

Tema custom este neobișnuit de curată pentru un magazin Shopify:

- **Fonturi de sistem** — zero fonturi web de descărcat.
- **Un singur fișier CSS** (`petshop-theme.css`, ~35 KB necomprimat, ~7 KB pe rețea).
- **Imaginile de produs/categorii/branduri** folosesc `srcset` + `sizes` +
  `loading="lazy"` + `width`/`height` — corect peste tot.
- **Iconițele** (trust badges, header, footer, social) sunt SVG inline, nu imagini.
- **Hero-ul are `preload` + `fetchpriority="high"`** — LCP-ul e tratat corect ca principiu.
- **GA4 / GTM / Meta Pixel nu au ID-uri configurate** în setările temei, deci
  scripturile de tracking din temă nici măcar nu se randează. Consent Mode e pregătit.

## Probleme găsite și corectate (în acest repo, `theme/`)

### 1. Srcset-ul hero-ului anunța variante inexistente (`sections/hero-banner.liquid`)

Imaginea hero curentă (`banner-v4.png`) are **1248 px lățime**, dar srcset-ul
(și preload-ul) declarau variante de `1600w` și `2000w`. CDN-ul Shopify nu
mărește imaginile, deci acele URL-uri returnează tot fișierul de 1248 px —
browserul își calcula greșit alegerea variantei. Fix:

- srcset generat dinamic, **limitat la lățimea reală a imaginii**, cu trepte
  mai fine pentru mobil (550/750/950/1200 px) — telefoanele descarcă varianta
  potrivită, nu una prea mare;
- `imagesizes="100vw"` adăugat explicit pe preload, ca preload-ul și `<img>`
  să aleagă garantat aceeași variantă (altfel imaginea se descarcă de două ori).

### 2. Bara promo producea layout shift (CLS) la vizitatorii care au închis-o (`sections/header.liquid`)

Verificarea `sessionStorage.getItem('promo-closed')` rula abia la finalul
întregului markup de header — pe mobil (CPU lent) bara apuca să se deseneze
și apoi dispărea, împingând tot conținutul în sus. Scriptul de ascundere a
fost mutat **imediat după elementul barei**, deci rulează înainte ca restul
paginii să fie desenat.

### 3. Cerere de rețea redundantă la fiecare încărcare (`sections/header.liquid`)

Header-ul făcea `fetch('/cart.js')` la fiecare pagină doar ca să scrie
numărul de produse din coș — care era **deja randat server-side** de Liquid
(`cart.item_count`). Cererea a fost eliminată; actualizările AJAX ale coșului
folosesc în continuare `updateCartCount()` din `layout/theme.liquid`.

### 4. Preconnect lipsă către CDN-ul Shopify (`layout/theme.liquid`)

Adăugat `<link rel="preconnect" href="https://cdn.shopify.com">` înaintea
CSS-ului — câștig mic, dar gratuit, pentru prima conexiune.

---

## Recomandări care NU țin de codul temei

### A. Imaginea hero — cel mai probabil elementul LCP pe mobil

`banner-v4.png`: **936 KB, PNG, 1248×832 px**. CDN-ul Shopify o convertește
la livrare în WebP/AVIF, dar un PNG fotografic rămâne o sursă proastă:

- **Re-exportă banerul la ~2000 px lățime, ca JPEG calitate ~80** (sau WebP)
  și încarcă-l în locul PNG-ului. Sursă mai bună ⇒ conversii CDN mai mici și
  mai clare pe desktop.
- Overlay-ul întunecat e la 65 % — dacă imaginea oricum abia se vede, o
  variantă mai simplă/mai comprimată a banerului dă același efect vizual la o
  fracțiune din cost.

### B. Sursele imaginilor de categorii

`c-ini.png`, `pisici…png`, `p-s-ri.png`, `roz-toare.png`, `fermieri.png`,
`promo-ii.png` au surse de **1–2 MB fiecare** (PNG 1024×1024). Nu se servesc
direct vizitatorilor (CDN-ul le redimensionează la 200–450 px), deci nu e
urgent — dar re-exportate ca JPEG/WebP ar da conversii mai mici și mai clare.

### C. GIF-uri vechi de sute de KB în bibliotecă

`phone_1.gif` (934 KB), `credit-card.gif` (680 KB), `truck_1.gif` (480 KB),
`sales.gif` (442 KB) — tema actuală nu le mai folosește (trust badges sunt
SVG acum), dar dacă apar în pagini statice sau descrieri de produs, ele nu
sunt optimizate de CDN la fel de bine. De înlocuit cu SVG/imagini statice
unde mai apar.

### D. Ce rămâne în afara controlului temei

Pe un magazin Shopify, o parte semnificativă din scorul mobil e consumată de
scripturile platformei (`content_for_header`: analytics intern, Shop Pay,
consent) și de app embed-uri — aici e activ **SE Delivery & Pickup (Sameday)**.
Acestea apar în PSI la „Reduce unused JavaScript" / „Third-party code" și nu
pot fi eliminate din temă. Merită verificat periodic în
Admin → Settings → Apps and sales channels ca app-urile neutilizate să fie
dezinstalate (nu doar ascunse), pentru că fiecare embed activ adaugă JS global.

---

## Cum se aplică fix-urile

Fișierele corectate sunt în `theme/` (diff-ul față de tema live este exact
diferența dintre primele două commit-uri). Ele **nu au fost urcate în tema
live** — pot fi aplicate prin Admin API la o confirmare, sau manual prin
copy/paste în Online Store → Edit code:

- `theme/layout/theme.liquid`
- `theme/sections/header.liquid`
- `theme/sections/hero-banner.liquid`

După aplicare + re-exportul banerului (punctul A), rulează din nou PageSpeed
pe mobil; îmbunătățirile așteptate sunt la **LCP** (imagine hero mai mică),
**CLS** (bara promo) și puțin la **FCP/TTFB percepute** (preconnect, o cerere
mai puțin).
