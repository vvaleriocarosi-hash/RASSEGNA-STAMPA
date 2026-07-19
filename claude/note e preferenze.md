# RASSEGNA STAMPA INGLESE — Note e preferenze del progetto

Questo file è il punto di partenza per qualsiasi nuova sessione Claude su questo progetto. Contiene tutto il contesto necessario: cos'è il progetto, come è fatto, le regole editoriali, il design system, lo schema dati e il flusso operativo con l'utente (Valerio).

## Istruzioni operative permanenti (leggere per prime)

1. All'inizio di ogni sessione su questo progetto, leggere questo file e `claude/worldmap-data.json` (snapshot dei dati più recenti) prima di fare qualunque cosa.
2. Il repo GitHub del progetto è: **https://github.com/vvaleriocarosi-hash/RASSEGNA-STAMPA**
3. Sul computer di Valerio (Windows, device "desktop-70pcnis") la cartella di lavoro locale è **`C:\Users\Valerio\Documents\GitHub\RASSEGNA-STAMPA`** — vero clone git, collegato a GitHub Desktop e al remote `origin` (branch `main`). Questa è la cartella ufficiale da usare, non la vecchia `Downloads\worldmap-repo` (che era solo una copia scaricata senza `.git`, ormai superata).
4. **Limite noto**: il bridge remote-devices in questa modalità (Cowork cloud) permette solo lettura/scrittura di file sul computer di Valerio, NON l'esecuzione di comandi shell/git. Quindi anche scrivendo i file aggiornati direttamente nella cartella locale, il commit/push finale su GitHub resta un'azione manuale di Valerio (es. con GitHub Desktop: due click "Commit" + "Push"). Non riproporre l'idea di un push completamente automatico a meno che non cambino gli strumenti disponibili.
5. Flusso di aggiornamento concordato (luglio 2026): preparare i file aggiornati (`data.json`, eventualmente `index.html`), scriverli direttamente nella cartella locale del repo via bridge quando disponibile, altrimenti inviarli in chat con SendUserFile. In entrambi i casi aggiornare anche la copia in `claude/worldmap-data.json` per mantenere lo snapshot di contesto aggiornato per le sessioni future.
6. Quando si aggiunge una notizia, cercare sempre almeno **due fonti indipendenti** (non un aggregatore che copia la prima) prima di pubblicarla — vedi sezione Fonti più sotto.
7. Ogni notizia va sempre fornita in **entrambe le lingue** (`en` e `it`) per `title`, `summary`, `detail` e `terms`.

## Cos'è il progetto

Pagina unica (home page) che mostra le notizie della rassegna stampa giornaliera in inglese: in alto una mappa del mondo interattiva con approfondimenti cliccabili per ogni paese, sotto una timeline a corsie per nazione con l'evoluzione nel tempo. Le notizie collegate tra loro (stessa storia che coinvolge più nazioni, es. un conflitto o un accordo commerciale) sono unite da linee tratteggiate colorate, sia sulla mappa sia sulla timeline. Il sito è pensato anche come strumento di studio dell'inglese: si apre di default in inglese, e ogni parola chiave evidenziata apre un approfondimento a lato.

## File del repository

- `index.html` — la pagina web completa: mappa 2D del mondo (D3.js) con marker per nazione/entità + sidebar di approfondimento, sotto la timeline a corsie con i punti cliccabili. Pagina completamente statica (HTML/CSS/JS), nessun backend. Il contorno del mondo e il font Poppins sono caricati da CDN esterni (jsDelivr, Google Fonts): se non disponibili la pagina resta comunque funzionante (feed, drawer, timeline), solo la mappa mostra un avviso al posto dello sfondo geografico.
- `data.json` — tutti i dati: `categories`, `categoryLabels` (bilingue), `places`, `specialTopics`, `deepdives` (bilingue), `items` (le notizie), `connections` (i legami tra notizie di nazioni diverse).
- `README.md` — istruzioni sintetiche di pubblicazione e aggiornamento (contenuto sovrapponibile a questo file, mantenuto per chi apre il repo su GitHub).
- `claude/note e preferenze.md` (questo file) e `claude/worldmap-data.json` — contesto di progetto per le sessioni Claude, non fanno parte del sito pubblicato.

## Design system

- Palette ispirata a "Internazionale": blu navy, rosso scuro, giallo, più due varianti derivate, per 6 colori categoria distinti:
  - Geopolitics `#8C1D24` (rosso scuro)
  - Economy `#F5C518` (giallo)
  - Technology `#1F3A5F` (blu navy)
  - Politics `#C1443B` (rosso, variante)
  - Social `#E0A106` (giallo, variante)
  - Medicine `#3D6690` (blu, variante)
- Titoli in **Avenir** (font di sistema, es. su Mac; fallback automatico a Poppins), testo in **Poppins** (Google Fonts).
- Menu categorie in alto ispirato ad ANSA.it: cliccando una categoria si filtra il feed di notizie.
- Feed stile "social network": card per ogni notizia con avatar/iniziali della nazione, tag categoria colorato, riassunto con parole chiave evidenziate in giallo.
- Cliccando una parola evidenziata (stile studenti.it) si apre un pannello laterale a destra ("drawer") con l'approfondimento, senza uscire dalla pagina. Le definizioni sono in `data.json → deepdives`; ogni notizia elenca in `terms` quali parole del `summary` vanno evidenziate e a quale `deepdive` puntano.
- Pulsante EN/IT in alto a destra nella topbar: passa da inglese a italiano senza ricaricare la pagina.

## Schema dati bilingue EN/IT

Ogni contenuto traducibile in `data.json` è annidato in due chiavi `en` e `it` (titoli, riassunti, approfondimenti, dossier, nomi di categorie e nazioni, testi delle connessioni). La lingua di default all'apertura è l'inglese, per aiutare lo studio della lingua; passando a IT si vede la stessa identica notizia tradotta.

Struttura di un `item` (notizia):

```json
{
  "id": "iran-strikes",
  "date": "2026-07-19",
  "country": "Iran",
  "category": "Geopolitics",
  "topics": ["us-iran"],
  "en": {
    "title": "...",
    "summary": "...",
    "detail": "...",
    "terms": [{ "match": "IRGC", "id": "irgc" }]
  },
  "it": {
    "title": "...",
    "summary": "...",
    "detail": "...",
    "terms": [{ "match": "IRGC", "id": "irgc" }]
  },
  "sources": [
    { "name": "CBS News", "url": "https://...", "reliability": 4 },
    { "name": "CNN", "url": "https://...", "reliability": 4 }
  ]
}
```

Una `connection` (legame tra notizie di nazioni diverse):

```json
{
  "from": "iran-strikes",
  "to": "jordan-attack",
  "category": "Geopolitics",
  "en": "Same conflict: ...",
  "it": "Stesso conflitto: ..."
}
```

Le connessioni si applicano soprattutto alle categorie Geopolitics, Economy e Social.

## Sistema delle due fonti indipendenti con punteggio di affidabilità

Ogni notizia in `data.json → items` ha un array `sources` con (di norma) **due fonti indipendenti** che hanno riportato lo stesso fatto, ciascuna con:

- `name`: nome della testata
- `url`: link all'articolo
- `reliability`: punteggio 1-5

Il punteggio `reliability` è una valutazione editoriale della **testata** (non della singola notizia), basata su: track record di accuratezza fattuale, presenza di standard editoriali/di correzione, uso di un linguaggio neutro e non partigiano.

- **5**: agenzie/enti ufficiali o servizi di cablaggio (Reuters, AP, BBC, NPR, PBS, dati governativi primari)
- **4**: grandi testate internazionali con standard editoriali solidi (Bloomberg, Washington Post, CNN, Al Jazeera, NBC, CBS News)
- **3 o meno**: testate minori, di parte, o con angolazione dichiaratamente nazionale/di categoria

Nel sito questo si vede come una fila di "pillole" cliccabili sotto ogni notizia (nel feed, nella finestra centrale e nella scheda paese), ciascuna con il nome della testata e un mini-rating a stelle.

**Regola editoriale**: quando si aggiunge una notizia, cercare sempre una seconda fonte realmente indipendente (non un aggregatore che copia la prima) prima di pubblicarla.

## Pubblicazione su GitHub Pages

1. Repository: `vvaleriocarosi-hash/RASSEGNA-STAMPA` su GitHub.
2. File nella root: `index.html`, `data.json`, `README.md`.
3. Settings → Pages, branch `main`, cartella `/root`.
4. URL pubblico confermato da Valerio: **https://vvaleriocarosi-hash.github.io/RASSEGNA-STAMPA/**

## Aggiornare la rassegna ogni giorno

Ad ogni rassegna stampa: aggiungere le nuove notizie a `data.json → items` (nuovo oggetto con `id` univoco, `date`, `country`, `category`, `en`/`it`, `sources`). Se compare una nazione nuova, aggiungere le sue coordinate in `places`. Se una notizia è collegata a un'altra in una nazione diversa, aggiungere un oggetto in `connections`. Poi commit/push (manuale, vedi sezione "Istruzioni operative permanenti"): GitHub Pages si aggiorna automaticamente.

## Cronologia decisioni rilevanti

- **19 luglio 2026**: verificato che il bridge remote-devices funziona (Windows, device "desktop-70pcnis") ma è limitato a lettura/scrittura file, senza esecuzione shell/git. Deciso di procedere con: scrittura diretta dei file aggiornati nella cartella locale via bridge, mentre Valerio fa commit/push manuale (con GitHub Desktop, due click). Non esisteva ancora una cartella `claude/` con questo file di note: ricreato in questa sessione a partire dal contenuto già presente in `README.md` e dalla struttura di `data.json`.
- **19 luglio 2026**: Valerio ha installato GitHub Desktop e clonato il repo in `C:\Users\Valerio\Documents\GitHub\RASSEGNA-STAMPA` (vero clone git). Fatto un test end-to-end: Claude ha scritto `test-connessione.md` via bridge, Valerio l'ha visto in GitHub Desktop e ha fatto commit + push. Da qui in avanti questa è la cartella ufficiale di lavoro (non più `Downloads\worldmap-repo`). Confermato anche l'URL pubblico del sito. Impostato un task programmato tutti i giorni alle 07:00 ora italiana (05:00 UTC — attenzione: il cron dei task programmati gira in UTC, non in ora locale) che prepara in anticipo le notizie del giorno.
- **19 luglio 2026 — bug fix pallini categoria**: in `index.html` la classe generica `.dot` (usata per i marker della timeline, con `position:absolute; top:50%`) si applicava per la cascata CSS (per-proprietà, non per-regola) anche ai puntini colorati delle categorie nel menu (`.cat-pill .dot`) e nel modal (`.modal-cat span.dot`), facendoli "volare" fuori posizione sopra la topbar — specialmente visibile su mobile. Fix: rinominata la classe della timeline in `.tl-dot` (CSS righe ~312/315 e `dot.className` nel JS del rendering timeline), lasciando `.dot` solo per i puntini inline accanto ai nomi delle categorie (menu, drawer, modal). **Regola per il futuro**: non riutilizzare mai il nome di classe generico `.dot` per nuovi elementi — usare nomi scoped tipo `.cat-dot`, `.tl-dot`, `.legend-dot`, ecc., perché in CSS le proprietà si applicano per-proprietà tra regole con specificità diversa, non "vince tutto o niente" la regola più specifica.
