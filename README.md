# Bacheca Parrocchiale Digitale - Frontend

Questo progetto contiene l'interfaccia utente (frontend) per la visualizzazione dinamica degli avvisi parrocchiali settimanali e del loro archivio storico. L'interfaccia è progettata per essere elegante, accessibile e completamente responsiva, con un'estetica premium basata su modalità scura (dark mode) e accenti dorati.

## Architettura

Il sito è un'applicazione web statica (HTML, CSS e JS Vanilla) ideale per essere ospitata su servizi come Cloudflare Pages. Interagisce in modo asincrono con un backend basato su **Cloudflare Workers** (`bacheca-worker`), il quale si occupa di gestire il caricamento dei file, l'elaborazione intelligente e l'esposizione delle API.

### Endpoint API Principali
Il frontend è configurato per comunicare con il worker ai seguenti endpoint:
- **`GET /api/latest`**: Restituisce i dati e il link all'immagine dell'avviso più recente (utilizzato in `index.html`).
- **`GET /api/calendar`**: *(In via di integrazione)* Restituisce in formato JSON l'elenco degli eventi imminenti (es. S. Messe, incontri) parsati dall'IA, comprensivi di titolo, data, luogo e orari.
- **`GET /api/history`**: Restituisce lo storico degli avvisi passati per popolare la griglia della pagina di archivio (`bacheca.html`).

## Struttura del Progetto

- **`index.html`**: La landing page principale. Presenta un layout split-screen su desktop con il foglio degli avvisi in grande evidenza. Include la logica per il fetching asincrono da `/api/latest` e la funzionalità per scaricare l'immagine originale.
- **`bacheca.html`**: La pagina di archivio storico. Mostra gli avvisi passati in una griglia ordinata. Implementa un **Lightbox personalizzato** in JS per visualizzare i fogli a schermo intero senza abbandonare la pagina.
- **`style.css`**: Il file che definisce il Design System dell'intero progetto.

## Design System & Interfaccia

L'esperienza visiva è progettata per trasmettere modernità e chiarezza, allontanandosi dai layout classici e datati:

- **Tipografia**: 
  - *Titoli*: `Playfair Display` (autorevole, classico, editoriale).
  - *Corpo del testo*: `DM Sans` (geometrico, moderno, altamente leggibile su schermi piccoli).
- **Palette Colori**:
  - *Sfondo*: Nero profondo (`#0F0D0B`)
  - *Testo*: Bianco opaco (`#F9F5EF`)
  - *Dettagli e Bottoni*: Oro (`#B8862A`) con variazioni più chiare al passaggio del mouse.
- **Micro-interazioni**: Animazioni fluide all'entrata in pagina (fade-in scaglionati) e feedback visivi sui bottoni e sulle miniature della bacheca (zoom e filtri colore).

## Sviluppo e Manutenzione

Il progetto adotta un approccio "Vanilla":
- **Zero Dipendenze**: Non sono utilizzati framework JavaScript (come React/Vue) né librerie CSS (come Bootstrap o Tailwind). Questo azzera i tempi di build e garantisce massima leggerezza.
- **Immagini su R2**: Il frontend assume che le immagini statiche degli avvisi siano servite tramite un bucket Cloudflare R2 collegato a un dominio personalizzato (es. `https://r2.lmetal.dev`).
- **Configurazione**: Gli URL delle API e del dominio R2 sono centralizzati all'inizio dei blocchi `<script>` all'interno dei file HTML per una facile modifica tra ambienti di test e produzione.
