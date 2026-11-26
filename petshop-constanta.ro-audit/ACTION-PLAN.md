# Plan de acțiune SEO — petshop-constanta.ro

Ordonat după impact. Efort: S = sub o oră, M = câteva ore, L = zile.

## Faza 1 — Săptămâna 1 (critic / ridicat)

| # | Acțiune | Unde | Efort | Impact |
|---|---|---|---|---|
| 1 | Publicați tema din repo (branch `claude/petshop-constanta-mobile-perf-tv3grl`) pe magazinul live, după o verificare în preview | Shopify Admin → Online Store → Themes | S | Repară `srcset`-ul hero și preload-ul dublu; LCP homepage estimat sub 3 s |
| 2 | Rescrieți cele 30 de titluri SEO duplicate (86 de produse) cu diferențiatorul: aromă, gramaj, culoare, mărime. Ex: „Luger's Conservă Câini Curcan & Broccoli 800g” | Admin → produs → Search engine listing | M | Elimină canibalizarea între variante |
| 3 | Corectați descrierea produsului Luger's vită & cartof 400g: subtitlurile H1 → H3 | Admin → produs → descriere | S | Un singur H1 pe pagină |

## Faza 2 — Săptămânile 2–3 (impact ridicat)

| # | Acțiune | Efort | Impact |
|---|---|---|---|
| 4 | Adăugați GTIN/EAN (câmpul „Barcode”) pe cele 344 de produse fără, și SKU pe cele 45 lipsă | M | Potrivire mai bună în Google Shopping / Merchant Center |
| 5 | Scurtați cele 121 de titluri de peste 60 de caractere | M | Titluri complete în SERP, CTR mai bun |
| 6 | Introducere de 100–200 de cuvinte pe cele 17 colecții subțiri (începeți cu pești, rozătoare, păsări) | M | Relevanță pentru interogările de categorie |
| 7 | Revizuiți aplicațiile și pixelii (Facebook, Pinterest, GTM, nfcube); dezinstalați ce nu folosiți | S | Reduce TBT (450 ms pe homepage) |

## Faza 3 — Luna 2 (conținut și autoritate)

| # | Acțiune | Efort | Impact |
|---|---|---|---|
| 8 | Plan editorial: 2 ghiduri/lună (ex. „Ce hrană alegi pentru un pui de câine”, „Acvariu pentru începători”, „Îngrijirea pisicii sterilizate”), cu link-uri către colecțiile relevante | L | Trafic informațional + citări în răspunsuri AI |
| 9 | Diferențiați titlurile celor două bloguri (`/blogs/news` vs `/blogs/ghiduri`) sau comasați-le | S | Elimină un duplicat |
| 10 | Evaluați gruparea produselor-variantă (11 culcușuri Taurus, 13 conserve Luger's) ca variante ale unui singur produs | L | Consolidează semnalele SEO și recenziile |
| 11 | Instalați o aplicație de recenzii; marcați `aggregateRating` doar pentru recenzii reale | M | Stele în rezultate + încredere |

## Faza 4 — Continuu

- Rulați din nou acest audit după publicarea temei și după rescrierea titlurilor, pentru a confirma.
- Conectați Google Search Console (și opțional GA4) pentru date reale de indexare, interogări și Core Web Vitals de teren.
- Verificați lunar descrierile duplicate la produsele noi (în special variantele).
