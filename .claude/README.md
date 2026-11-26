# Configurație Claude Code pentru acest repo

## Plugin: claude-seo

Pluginul open-source [`claude-seo`](https://github.com/AgriciDaniel/claude-seo) (v2.2.4) este
vendorizat aici, la nivel de proiect, în `.claude/plugins/claude-seo/`, și înregistrat ca
marketplace local + plugin activat în `.claude/settings.json`. Oricine deschide acest repo cu
Claude Code îl are disponibil automat — nu necesită instalare separată.

Ce oferă: 25 de sub-skill-uri și 18 sub-agenți pentru SEO tehnic, conținut, schema markup,
sitemap-uri, Core Web Vitals, SEO local, backlinks, AI/GEO, ecommerce, hreflang, SXO, clustering
și monitorizare de drift, plus extensii MCP opționale (DataForSEO, Firecrawl, Ahrefs, SE Ranking,
Profound, Bing Webmaster, Unlighthouse, Banana).

Utilizare de bază: `/seo audit https://petshop-constanta.ro` (vezi `.claude/plugins/claude-seo/README.md`
pentru documentația completă).

Notă: unele skill-uri/scripturi ale pluginului necesită un runtime Python 3.10+ și, opțional,
chei API pentru serviciile externe (Google Search Console, DataForSEO etc.) — acestea se
configurează separat, la nevoie; nu sunt incluse în acest repo. Directoarele `tests/`,
`screenshots/`, `.github/` și `.devcontainer/` din sursa originală a pluginului au fost omise
la vendorizare (nu sunt necesare la runtime).

De reținut: hook-ul `PostToolUse` din `hooks/hooks.json` al pluginului (validare schema markup
după Edit/Write) rulează doar dacă pluginul este efectiv activat de Claude Code prin mecanismul
de marketplace de mai sus — copierea manuală a fișierelor nu e suficientă pentru asta.

Pentru a dezactiva temporar pluginul doar pentru tine (fără să afectezi restul echipei), adaugă
în `.claude/settings.local.json`:

```json
{ "enabledPlugins": { "claude-seo@agricidaniel-claude-seo": false } }
```
