# AGENTS.md ? stupina.html / index.html

Acest fi?ier descrie structura `stupina.html` ?i regulile de lucru pentru modific?ri. `index.html` este copia principal? pentru GitHub Pages ?i trebuie ?inut sincron cu `stupina.html`.

## Reguli obligatorii la fiecare modificare

- **Backup ?nainte de modificare**: creeaz? o copie de backup ?n `.tmp/` ?nainte de a edita `stupina.html` sau `index.html`.
- **Actualizeaz? AGENTS.md**: dup? fiecare modificare, actualizeaz? intervalele/zonele afectate.
- **P?streaz? func?ionalit??ile existente**: design cald, meniu de jos, `localStorage` + Export/Import JSON, meteo, fi?? stup, audio, camer?, jurnal.
- **Validare**: verific? sintaxa JS (`node --check` pe scripturile extrase) ?i deschide pagina ?n browser.

## Hart? fi?ier `stupina.html` (orientativ, ~6400 linii)

| Interval | Con?inut |
|---:|---|
| 1-13 | Head/meta/title/fonturi |
| 14-~800 | CSS principal: meteo, monitorizare, declarant/facturi/calendar/re?etar, tablet? digital?, checklist vizit?, hart? stupin?, etichete borcane, unelte extra |
| ~800-~1260 | Body/header/meteo/main: taburi Stupi/Tratamente/Hr?nire/Recolte/Magazie/QR & Unelte |
| ~1260-3546 | HTML module + Script 1 QR inline |
| 3548-eof | Script 2 logica aplica?iei |

## Zone HTML v5

| Zon? | Rol |
|---|---|
| Tab Stupi | Toggle `List?`/`Hart? Stupin?`, `#nextVisitPrep`, `#hiveMap` |
| Fi?? stup | Tableta digital? (5 st?ri: Botc?/Transvazare, Matc? t?n?r?, Zbor ?mperechere, Matc? ?mperecheat?/Familie OK, Depopulat/Necesit? ajutor), lot `S-10`, dat?, eclozionare ziua 12; checklist rapid vizit? 4 toggle-uri; performan?? matc? 2026 (Bl?nde?e/Igien?/Produc?ie miere/Tendin?? roire, 1-5) |
| Tab Recolte | Formular etichete borcane + `Genereaz? Etichete Borcane (A4 Print)` (Miel de Fleurs/Acacia/Tilleul/Ch?taignier, 1000g/500g, dat? recolt?, lot, DDM +2 ani, men?iuni Fran?a, produc?tor ILIE NICOARA NAPI A5200524) |
| Tab QR & Unelte | `Unelte Utile Extra`: Eviden?? Auto & Utilitare (CT/Asigurare/Revizie km/jurnal repara?ii), Calculator Devize & Materiale Fran?a (TVA 10/20%, `Devis estimatif`), ?abloane Scrisori Oficiale Fran?a (Mairie/Assurance/Imp?ts/CPAM-Ameli) |

## JS - model/date

| Func?ie / zon? | Rol |
|---:|---|
| Constante | `KEY`, `WEATHER_*`, `DECLARANT` NAPI A5200524/CERFA 13995*07, `HIVE_COLORS`, `QUEEN_ORIGINS`, `QUEEN_BREEDS`, `ICKO_2026`, `FEED_RECIPES`, `CALENDAR_IDF`, `TABLET_STATES` |
| `emptyDB()` | DB: hives/treatments/feedings/harvests/stock/expenses/sales/invoices/vehicles/devis/letters |
| `createStandardHive(n)` | Stup 2026 + `tabletState/tabletLot/tabletDate/queenScores/visitChecklist/visitChecklistDate/nextVisitPrep` |
| `normalizeDB()` | Normalizeaz? DB vechi/nou; compatibil Export/Import; asigur? c?mpuri v5 |
| `exportJSON()/importJSON()/resetAll()` | Backup/restore/reset localStorage |

## JS - v5

| Func?ie | Rol |
|---:|---|
| `tabletStateMeta/tabletTag/eclozionDay` | 5 st?ri, etichet? `S-10 ? 21.04.2026`, eclozionare +12 zile |
| `renderHiveTabletUI/renderVisitChecklistUI/renderQueenScoresUI` | UI fi?? stup: stare, lot/dat?, checklist vizit?, scoruri matc? 1-5 |
| `setHiveViewMode/renderHiveMap/renderNextVisitPrep` | List?/Hart?, culori reale l?zi, scor mediu matc?, sumar preg?tire vizit? urm?toare |
| `printHoneyLabels` | A4 8 etichete/pagin?, men?iuni obligatorii Fran?a, DDM +2 ani |
| `renderVehicles/addVehicle` | Eviden?? auto/utilitare ?n `db.vehicles` |
| `renderDevis/addDevisLine/printDevis` | Devize/materiale TVA 10/20%, print `Devis estimatif` |
| `printLetter` | Scrisori Mairie/Assurance/Imp?ts/CPAM auto-completate cu declarant |

## JS - func?ionalit??i p?strate

- Meteo Open-Meteo + cache `stupina.weather`.
- Fi?? stup: climat 33-36?C, SVG chart, audio FFT 10s/simulare, camer?, urdini?/Vespa jurnal, contoare, galerie foto, interven?ii.
- Dictare vocal? Web Speech API `ro-RO`.
- ICKO 2026: 7 facturi, 396,83? TTC, anti-duplicat MBUC, auto/manual load.
- Calendar ?le-de-France + beep o dat?/zi, re?etar/calculator 7 stupi.
- Registre d'?levage print/PDF CERFA.
- QR, scan/import, export/import/reset JSON.

## Jurnal modificări — 2026-11-24

| Element | Detaliu |
|---|---|
| `stupina.html` | 6142 linii; `index.html` copie identică după reparare |
| Backup înainte de fix | `.tmp/stupina_backup_pre_fix.html`, `.tmp/index_backup_pre_copy.html`, `.tmp/agents_backup_pre_fix.md` |
| Cauza erorii JS | Identificatori lipsă: `TABLET_STATES`, `$all`, `todayISO`, `parseDate`; `init()` se oprea înainte de `loadWeather()` |
| Reparații JS | `TABLET_STATES` la linia 3606; `$all` la 3790; `todayISO` la 3812; `parseDate` la 3820; culorile hărții stupinii corectate la 5303; câmpul `an matcă` folosește `h.year` în `hiveCard` |
| Diacritice | 245 de caractere `?` corupte restaurate în UTF-8; cele 155 rămase sunt operatori ternari, confirmări sau parametrizi URL legitimi |
| Verificare | `node --check` OK pentru ambele scripturi; test de execuție JS: 7 stupi randati, 7 în DB, meteo „Parc de la Saussaie” randat cu date Open-Meteo |
