# Science for Benefit — Dashboard sui Rischi Climatici / Climate Risk Dashboard

Dashboard interattiva per esplorare gli indicatori climatici storici (baseline) e confrontare le proiezioni locali e globali secondo gli scenari IPCC (**Historical 1850–2014**, **SSP1-2.6**, **SSP2-4.5**, **SSP3-7.0**, **SSP5-8.5**).

---

## 1. Come usare la dashboard web (`index.html`)

La versione statica (`index.html`) funziona direttamente in qualsiasi browser (PC, Mac, smartphone o tablet) **senza bisogno di installare R**.

### Avvio rapido
- **Online (GitHub Pages):** apri il link GitHub Pages del repository (vedi [README_GITHUB.md](README_GITHUB.md)).
- **In locale:** fai doppio clic su `index.html` per aprirlo nel browser (Chrome, Edge, Firefox, Safari).

> **Nota sulla copertura dati:** `index.html` incorpora già tutte le mappe globali, l'elenco dei comuni italiani e le griglie locali dei 5 scenari per **tutta l'Italia e l'Europa** (`33°–61°N`, `12°W–36°E`). Se mantieni la cartella `web_data/grids/` accanto a `index.html`, la dashboard estrae i valori locali anche per **qualsiasi altra coordinata nel resto del mondo**.

### Guida alle funzionalità

1. **Selezione dell'indicatore climatico (pannello sinistro)**
   - Dal menu a tendina **Selezionare l'indicatore di rischio climatico**, scegli uno dei **27 indicatori climatici** (es. *Consecutive dry days*, *Frost days*, *Maximum 1-day precipitation*, *Tropical nights*, *WSDI*, ecc.).
   - Sotto il selettore viene mostrata automaticamente l'unità di misura (`Unità: day`, `°C`, `mm`, ecc.).

2. **Selezione della località / coordinate**
   Puoi impostare il punto geografico in **4 modi**:
   - **Ricerca per comune italiano:** digita almeno 2 lettere nel campo **Cerca comune italiano** (es. `Brescia`, `Roma`, `Milano`) e seleziona il comune dal menu a discesa.
   - **Pulsanti rapidi e GPS:** usa i pulsanti rapidi (`Roma`, `Brescia`, `Milano`, `Napoli`) oppure clicca **📍 Posizione** per rilevare automaticamente le coordinate GPS del dispositivo.
   - **Inserimento manuale:** digita direttamente **Latitudine** e **Longitudine** nei rispettivi campi numerici.
   - **Click diretto sulla mappa:** clicca o tocca un qualsiasi punto sulla mappa interattiva per spostare il segnaposto e calcolare i dati locali.

3. **Lettura della mappa interattiva (Baseline storica)**
   - La mappa mostra la distribuzione globale dell'indicatore selezionato nel periodo storico (`Historical 1850–2014`).
   - **Scala cromatica:** blu (`#2D5D9F`) indica valori inferiori alla media storica globale; bianco indica la media globale; rosso (`#C0392B`) indica valori superiori alla media.
   - Cliccando sul segnaposto blu si apre un popup con le coordinate esatte e il valore storico locale.

4. **Lettura del grafico e della tabella "Confronto tra scenari"**
   Il pannello inferiore confronta la distribuzione globale dell'indicatore nei 5 scenari climatici rispetto al valore locale del punto scelto:
   - **Barre azzurre (Fasce percentile):** mostrano l'intera distribuzione globale dal minimo al massimo attraverso 6 fasce (`0–10%`, `10–25%`, `25–50%`, `50–75%`, `75–90%`, `90–100%`).
   - **Quadrato blu scuro (`■`):** Media globale dello scenario.
   - **Rombo blu (`◆`):** Mediana globale (`p50`) dello scenario.
   - **Cerchio (`●`):** Valore locale esattamente alle coordinate selezionate (blu scuro per `Historical`, azzurro chiaro per gli scenari `SSP`).
   - **Tabella riepilogativa:** riporta i valori numerici esatti per ogni scenario (*Valore locale*, *Media globale*, *Mediana p50* e *Intervallo p10–p90*).

---

## 2. Come eseguire la versione R Shiny (`app.R`)

Se desideri avviare la versione originale in R Shiny sul tuo computer:

1. Apri R o RStudio nella cartella `ScienceForBenefit`.
2. Assicurati di avere i pacchetti richiesti: `shiny`, `tidyverse`, `bslib`, `openxlsx`, `leaflet`, `terra`.
3. Esegui:
   ```r
   shiny::runApp("app.R")
   ```

---

## 3. Come rigenerare `index.html` e `web_data/` da R (`build_static_web.R`)

Se aggiorni i file NetCDF (`*.nc`), gli shapefile dei comuni (`maps/`) o il logo (`www/logo.jpg`), puoi rigenerare `index.html` e `web_data/` in qualsiasi momento:

- **Da R / RStudio:**
  ```r
  source("build_static_web.R")
  ```
- **Da terminale (PowerShell / CMD):**
  ```powershell
  Rscript build_static_web.R
  ```

Lo script elabora tutti i 27 indicatori × 5 scenari (135 file `.nc`), estrae i centroidi dei comuni italiani e genera:
- `index.html` (dashboard autonoma completa)
- `web_data/meta.js` (metadati, mappe raster e sub-griglie Europa/Italia)
- `web_data/comuni.js` (elenco comuni italiani e coordinate)
- `web_data/grids/*.js` (griglie globali complete per ogni indicatore)
