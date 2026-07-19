# World News Map & Timeline — Rassegna Stampa Inglese

Pagina unica (home page) che mostra le notizie della rassegna stampa giornaliera: in alto una mappa del mondo interattiva con approfondimenti cliccabili per ogni paese, sotto una timeline a corsie per nazione con l'evoluzione nel tempo. Le notizie collegate tra loro (stessa storia che coinvolge più nazioni, es. un conflitto o un accordo commerciale) sono unite da linee tratteggiate colorate, sia sulla mappa sia sulla timeline.

## Come funziona

- `index.html` — la pagina web completa: mappa 2D del mondo (D3.js) con marker per nazione/entità + sidebar di approfondimento, e sotto la timeline a corsie con i punti cliccabili.
- `data.json` — i dati: categorie/colori, `places` (coordinate approssimative di ogni nazione/entità), `items` (le notizie: id, data, paese, categoria, titolo, riassunto, fonte, link), `connections` (i legami tra notizie di nazioni diverse: `from`, `to`, `category`, `label`).

Il colore di ogni punto/categoria è definito in `data.json` sotto `categories`. Le connessioni tra nazioni si applicano soprattutto alle categorie Geopolitics, Economy e Social.

## Pubblicare su GitHub Pages

1. Crea un nuovo repository su GitHub (es. `rassegna-stampa-map`).
2. Carica i tre file di questa cartella (`index.html`, `data.json`, `README.md`) nella root del repository.
3. Vai su **Settings → Pages**, seleziona il branch `main` e la cartella `/root`, poi salva.
4. Dopo qualche minuto la mappa sarà visibile su `https://<tuo-utente>.github.io/<nome-repo>/`.

## Aggiornare la mappa ogni giorno

Ad ogni rassegna stampa, aggiungi le nuove notizie a `data.json` (nuovo oggetto dentro `items`, con `id` univoco, `date`, `country`, `category`, `title`, `summary`, `source`, `url`). Se compare una nazione nuova, aggiungi anche le sue coordinate in `places`. Se una notizia è collegata a un'altra in una nazione diversa, aggiungi un oggetto in `connections` (`from`, `to`, `category`, `label`). Poi fai commit/push: GitHub Pages si aggiorna automaticamente.

## Fonti multiple e punteggio di affidabilità

Ogni notizia in `data.json → items` ha ora un array `sources` con (di norma) due fonti indipendenti che hanno riportato lo stesso fatto, invece di un singolo campo `source`/`url`: `"sources": [{"name": "...", "url": "...", "reliability": 1-5}, {...}]`. Il punteggio `reliability` (1-5) è una valutazione editoriale della testata — non della singola notizia — basata su: track record di accuratezza fattuale, presenza di standard editoriali/di correzione, e uso di un linguaggio neutro e non partigiano. Fonti come agenzie/enti ufficiali o servizi di cablaggio (Reuters, AP, BBC, NPR, PBS, dati governativi primari) tendono a 5; grandi testate internazionali con standard editoriali solidi (Bloomberg, Washington Post, CNN, Al Jazeera, NBC) a 4; testate minori, di parte o con angolazione dichiaratamente nazionale/di categoria a 3 o meno. Nel sito questo si vede come una fila di "pillole" cliccabili sotto ogni notizia (nel feed, nella finestra centrale e nella scheda paese), ciascuna con il nome della testata e un mini-rating a stelle. Quando si aggiunge una notizia, cercare sempre una seconda fonte realmente indipendente (non un aggregatore che copia la prima) prima di pubblicarla.

## Versione bilingue (EN/IT)

Il sito ha un pulsante EN/IT in alto a destra nella topbar: cliccandolo si passa dall'inglese all'italiano senza ricaricare la pagina. Ogni contenuto traducibile in `data.json` è annidato in due chiavi `en` e `it` (titoli, riassunti, approfondimenti, dossier, nomi di categorie e nazioni, testi delle connessioni). La lingua di default all'apertura è l'inglese (`en`), per aiutare lo studio della lingua; passando a IT si vede la stessa identica notizia tradotta in italiano. Aggiungendo una nuova notizia a `data.json`, va sempre fornita sia la versione `en` sia la versione `it` dei campi `title`, `summary`, `detail` e `terms`.

## Design

- Palette ispirata a "Internazionale": blu navy, rosso scuro, giallo (più due varianti derivate per avere 6 colori categoria distinti).
- Titoli in **Avenir** (font di sistema, se installato — es. su Mac; altrimenti fallback automatico a Poppins), testo in **Poppins** (caricato da Google Fonts).
- Menu categorie in alto ispirato ad ANSA.it: cliccando una categoria si filtra il feed di notizie.
- Feed stile "social network": card per ogni notizia con avatar/iniziali della nazione, tag categoria colorato, riassunto con parole chiave evidenziate in giallo.
- Cliccando una parola evidenziata (stile studenti.it) si apre un pannello laterale a destra ("drawer") con l'approfondimento — senza uscire dalla pagina. Le definizioni sono in `data.json` sotto `deepdives`; ogni notizia elenca in `terms` quali parole del suo `summary` vanno evidenziate e a quale `deepdive` puntano.

## Note tecniche

- La pagina è completamente statica (HTML/CSS/JS), nessun backend necessario.
- Il contorno del mondo (mappa) e il font Poppins vengono caricati al volo da CDN esterni (jsDelivr, Google Fonts): servono per vederli renderizzati al meglio, ma se non disponibili la pagina resta comunque funzionante — feed, drawer di approfondimento e timeline funzionano anche offline dai CDN, la mappa mostra solo un avviso al posto dello sfondo geografico.
