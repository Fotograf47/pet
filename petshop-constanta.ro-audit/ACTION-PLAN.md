# Plan de acțiune SEO — petshop-constanta.ro

Ordonat după impact. Efort: S = sub o oră, M = câteva ore, L = zile.

## Faza 1 — Săptămâna 1 (critic / ridicat)

| # | Acțiune | Unde | Efort | Impact |
|---|---|---|---|---|
| 1 | Publicați copia de temă „Preload hero fix 2026-11-26” (deja pregătită și verificată). **Nu publicați tema din repo**: e mai veche decât tema activă | Shopify Admin → Online Store → Themes | S | Elimină descărcarea dublă a imaginii hero; LCP mai mic pe homepage |
| 2 | Înlocuiți trunchierea la 42 de caractere a titlurilor de produs (blocul `<title>` din `layout/theme.liquid`) cu o regulă care păstrează diferențiatorul (aromă, gramaj, culoare, mărime) | Tema activă (cod) | S–M | Rezolvă cele 30 de grupuri de titluri duplicate (86 de produse) dintr-o singură modificare |
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
