# Documentazione sito — Famiglia De Angelis S.r.l.

> Documento di riferimento completo del sito web. Pensato per essere condiviso con
> un assistente AI (es. Gemini) o con uno sviluppatore che debba capirlo o modificarlo,
> senza dover leggere il codice.

---

## 1. Sintesi del progetto

| Voce | Valore |
|---|---|
| Cliente | **Famiglia De Angelis S.r.l.** (storicamente "CineArs") |
| Attività | Laboratorio di scultura e scenografia cinematografica agli Studi di Cinecittà, Roma. Dal 1919, 4 generazioni. |
| Obiettivo del sito | Landing page unica orientata alla conversione: mostrare autorevolezza/heritage e raccogliere richieste di progetto tramite un modulo. |
| Target | Produzioni cinema/TV, scenografi e art director, studi di interior/eventi, musei, collezionisti e privati. |
| Tono / stile | Minimal, scuro ("dark cinema"), elegante, autorevole. Grandi immagini, tipografia pulita, dettagli oro. |
| Lingua | Italiano |
| Stato | Prototipo funzionante online. Contenuti e immagini in gran parte segnaposto (vedi §7). |

**Live:** `https://famiglia-de-angelis-srl.vercel.app`
**Repository:** `https://github.com/leonardopanichella08-dotcom/FamigliaDeAngelisSRL`
**Deploy:** GitHub → Vercel (deploy automatico a ogni push su `main`).

---

## 2. Architettura tecnica

- **Un unico file:** `index.html` — sito statico, nessun backend, nessun build step.
- **CSS:** Tailwind CSS caricato da CDN (`cdn.tailwindcss.com`) + un blocco `<style>` interno per le parti custom (glass, animazioni, sfondo).
- **Font:** Google Fonts — *Playfair Display* (display/titoli) e *Inter* (testo/UI).
- **3D:** Three.js r128 (da jsDelivr) + `GLTFLoader`. Il modello 3D (statua del Discobolo) è incorporato direttamente nell'HTML come dato binario in base64 (~3,5 MB), quindi il file `index.html` pesa circa 4,7 MB.
- **Modulo contatti:** [FormSubmit.co](https://formsubmit.co) via chiamata AJAX (`fetch`). Nessun account: le richieste arrivano via email a `leonardopanichella08@gmail.com`. Serve una conferma email una tantum al primo invio.
- **Dipendenze esterne a runtime:** CDN Tailwind, Google Fonts, jsDelivr (Three.js), API FormSubmit, immagini da Wikimedia Commons (in hotlink). Il sito richiede connessione internet per rendersi correttamente.

---

## 3. Sistema visivo (UI / Art direction)

### Palette
| Ruolo | Colore | Note |
|---|---|---|
| Fondo | `#0d0d0d` (near-black) | Fondo unico e continuo per tutta la pagina |
| Fondo alt. | `#141414` / `#1a1a1a` | Pannelli, card |
| Linee | `#2b2b2b` | Bordi sottili |
| Accento | `#d4af37` (oro) | Pulsanti, dettagli, numeri, occhielli |
| Accento chiaro | `#e8cd7a` | Hover, titoli material card |
| Accento tenue | `#C5A059` | Occhielli, didascalie |
| Testo | bianco → `gray-300/400/500` | Gerarchia per contrasto decrescente |

### Tipografia
- **Titoli / display:** *Playfair Display* (serif elegante, richiamo classico/scultoreo). Pesi 600–800. `text-wrap` bilanciato sui titoli.
- **Testo e UI:** *Inter* (sans). Pesi 300–600.
- **Occhielli:** maiuscolo, corpo molto piccolo (`text-xs`), `letter-spacing` ampio (0.3em), colore oro tenue.
- **Scala titoli:** hero fino a ~3.75rem (desktop); titoli sezione ~2.25rem; sottotitoli card ~1.25rem.
- **Numeri statistiche:** `tabular-nums` + ombra scura per staccarli dallo sfondo.

### Layout & spaziatura
- Contenitore centrato `max-width` ~80rem (`max-w-7xl`), padding orizzontale `20px` mobile / `32px` desktop.
- Ritmo verticale generoso: `96px` (mobile) / `128px` (desktop) tra le sezioni.
- Griglie responsive: 1 colonna mobile → 2/3/4 colonne desktop.
- Hero alto `92vh` (min 640px), contenuto ancorato in basso.
- Header fisso in alto, altezza 80px, sfondo nero trasparente + `backdrop-blur`; niente bordo; ombra che compare allo scroll.

### "Liquid glass"
Pannelli in vetro smerigliato usati per: barra statistiche, card servizi, pillola dei filtri portfolio, contenitore del modulo.
Caratteristiche: sfondo bianco molto trasparente (8–15%), `backdrop-filter: blur(20–28px) saturate(160%)`, bordo chiaro `rgba(255,255,255,0.3)`, riflesso chiaro sul bordo superiore (inset), ombra interna in basso, angoli molto arrotondati (18–28px). **Nessuna animazione "metallica" sopra il vetro** (rimossa su richiesta): il vetro è statico e pulito, la vita la danno lo sfondo e le foto sotto.

### Sfondo animato continuo
Un livello **fisso** (`position: fixed`) dietro tutta la pagina con 5 "bagliori" (`radial-gradient` oro/ambra) grandi e molto sfocati (`blur(80px)`), che derivano lentamente (cicli 8–15s) con leggera scala. Restano visibili in modo identico dall'inizio alla fine dello scroll → sfondo percepito come un unico ambiente, senza cuciture tra sezioni.

### Trattamento delle immagini
- **Foto full-bleed** in Hero, Servizi, Modulo: coprono tutta la sezione, scurite pesantemente con gradienti/scrim, con i **bordi in dissolvenza** (mask verticale ~18%/82%) che si fondono nello sfondo continuo.
- **Foto in card** (portfolio, materiali, magazine, chi siamo): "cornici" uniformi con angoli arrotondati (~18px), rapporto **4:5**, `object-fit: cover`, overlay scuro in basso, barra didascalia in vetro, tag categoria in alto a sinistra. Leggero zoom della foto all'hover.

### Pulsanti e controlli
- **Pulsanti a pillola** (`border-radius: 9999px`), stile "Apple": micro-rimbalzo elastico all'hover (`translateY(-2px) scale(1.03)`), leggera compressione al click.
- Pulsante oro pieno: riflesso lucido ("gloss") sul bordo superiore.
- Pulsante secondario: solo contorno (oro o bianco), si colora all'hover.
- **Tessere icona** (servizi, badge 01/02): quadrato con angoli arrotondati (`rounded-xl`), bordo oro; su hover della card si riempiono d'oro e ruotano/ingrandiscono di poco.
- **Icone:** stile linea sottile (tipo Heroicons), stroke 1.5.
- Campi del modulo: fondo nero semitrasparente, bordo `#2b2b2b`, angoli `rounded-lg`, bordo oro al focus.
- Barra filtri portfolio: su mobile diventa una **riga a scorrimento orizzontale** (niente a-capo), così il contenitore resta sempre una pillola pulita.

### Movimento (tutto disattivato se `prefers-reduced-motion`)
| Elemento | Effetto |
|---|---|
| Sezioni | Fade-up all'ingresso in viewport (con ritardi a cascata sulle griglie) |
| Statistiche | Conteggio da 0 al valore |
| Striscia film | Marquee orizzontale in loop infinito (pausa all'hover) |
| Foto di sfondo | Parallax legato allo scroll + lento pan/zoom (Ken Burns), più marcato sull'hero (ciclo 18s) |
| Hero | Luci calde che "tremano" (come tra le fronde) + 3 sagome scure sfocate che attraversano in lontananza |
| Statua 3D | Ruota sull'asse Y in funzione della posizione di scroll della pagina |
| Sfondo | Bagliori che derivano di continuo |

---

## 4. Mappa delle sezioni e testi (nell'ordine)

### HEADER (fisso)
- Logo: **FAMIGLIA DE ANGELIS** — sottotitolo **CINESCULTURA DAL 1919**
- Menu: Chi Siamo · Servizi · Portfolio / Filmografia · Materiali · Magazine · Contatti
- CTA: **Richiedi un Progetto**
- Mobile: menu hamburger a comparsa (stessi link + CTA)

### 1. HERO (`#top`)
- Sfondo: foto Foro Romano a tutta pagina (animata)
- Occhiello: *Studi di Cinecittà — Roma*
- Titolo: **DIAMO FORMA ALL'IMMAGINAZIONE.**
- Testo: *"Da oltre un secolo realizziamo calchi, statue e scenografie per i capolavori del cinema italiano e internazionale negli Studi di Cinecittà."*
- CTA: **Racconta il tuo progetto** · **Esplora la Filmografia**

### 2. STATISTICHE (pannello vetro fluttuante)
| Valore | Etichetta |
|---|---|
| 100+ | Anni di Attività (Dal 1919) |
| 4 | Generazioni di Scultori |
| 20+ | Grandi Produzioni Internazionali |
| 300+ | Opere in Mostre Nazionali |

### 3. STRISCIA FILMOGRAFIA (marquee)
`Ben-Hur · Il Gladiatore · La Dolce Vita · Gangs of New York · The Young Pope · + 20 Produzioni`

### 4. CHI SIAMO (`#chi-siamo`)
- Occhiello: *Chi Siamo* — Titolo: **L'eredità della quarta generazione**
- P1: *"Dal 1919, la Famiglia De Angelis tramanda un mestiere raro: la scultura scenografica applicata al cinema. Quattro generazioni si sono succedute nello stesso laboratorio, negli Studi di Cinecittà, portando avanti una tradizione artigianale che ha attraversato la storia del cinema italiano e internazionale."*
- P2: *"Oggi il laboratorio unisce le tecniche classiche della scultura — modellazione, formatura, calco — alle esigenze contemporanee dell'industria audiovisiva, lavorando fianco a fianco con scenografi, art director e produzioni di tutto il mondo."*
- **01 · Artigianato classico** — *Tecniche tramandate da oltre 100 anni, eseguite interamente a mano.*
- **02 · Industria dell'audiovisivo** — *Tempistiche e standard produttivi pensati per set e produzioni internazionali.*
- Immagini: "Prima generazione — Roma, primi del '900" / "Laboratorio oggi — IV generazione"

### 5. CITAZIONE + STATUA 3D
- Sinistra: **statua 3D del Discobolo** (canvas trasparente, nessuno sfondo) che ruota con lo scroll. Didascalia: *"Discobolo · scansione 3D, dominio pubblico (SMK — Statens Museum for Kunst)"*.
- Destra: *"Ogni statua racconta una scena. Ogni scena, un secolo di mestiere."* — **Famiglia De Angelis — IV Generazione, Cinecittà**

### 6. SERVIZI (`#servizi`) — 4 card in vetro su foto Cinecittà
- Occhiello: *I Nostri Servizi* — Titolo: **"Dal bozzetto al set, ogni fase in un unico laboratorio"**

| Servizio | Descrizione |
|---|---|
| **Calchi e Statue** | Gesso, resina, vetroresina e bronzo. Riproduzioni fedeli e opere originali per set e collezioni. |
| **Scenografie & Set Design** | Realizzazione completa da bozzetto o concept, per produzioni cinematografiche, TV ed eventi. |
| **Restauro Scenografico** | Restauro artistico e conservativo di opere, calchi storici e allestimenti museali. |
| **Noleggio Props** | Oggetti di scena iconici disponibili per noleggio, catalogati e pronti per la produzione. |

### 7. PORTFOLIO / FILMOGRAFIA (`#filmografia`)
- Occhiello: *Portfolio* — Titolo: **Filmografia & Progetti**
- Filtri: **Tutti · Cinema & TV · Eventi & Design · Restauri**

| # | Categoria | Titolo | Sottotitolo |
|---|---|---|---|
| 1 | Cinema & TV | Il Gladiatore | Statuaria romana in vetroresina |
| 2 | Cinema & TV | The Young Pope | Calchi e busti scenografici |
| 3 | Cinema & TV | Ben-Hur | Scenografia dell'arena e statuaria |
| 4 | Cinema & TV | Gangs of New York | Ricostruzione scenografica d'epoca |
| 5 | Cinema & TV | La Dolce Vita | Elementi scultorei di scena |
| 6 | Eventi & Design | Allestimento Privato — Roma | Sculture su misura per interni |
| 7 | Eventi & Design | Evento Corporate — Milano | Installazione scenografica temporanea |
| 8 | Restauri | Busto Storico — Collezione Privata | Restauro conservativo in gesso |
| 9 | Restauri | Statua da Museo | Restauro artistico su calco storico |

### 8. MATERIALI E TECNICHE (`#materiali`)
- Occhiello: *Materiali e Tecniche* — Titolo: **"Dal negativo al positivo. Dal bozzetto alla scena."**
- Testo: *"Ogni opera nasce da un processo artigianale che unisce disegno, modellazione e formatura, prima di essere tradotta nel materiale definitivo scelto per la scena."*
- **Gesso** — *Il materiale classico della scultura, per calchi, prove e opere da set.*
- **Resina & Vetroresina** — *Leggerezza e resistenza per props e statuaria da set.*
- **Bronzo** — *Fusioni artistiche per opere permanenti e commissioni museali.*

### 9. MAGAZINE (`#magazine`)
- Occhiello: *Magazine* — Titolo: **Chicche di Cinema**
- Anteprime (senza contenuto reale): "I segreti del set di Ben-Hur" (*Dietro le quinte*) · "Come nasce una statua da film" (*Artigianato*) · "Curiosità sui props di scena" (*Curiosità*)

### 10. MODULO "RICHIEDI UN PROGETTO" (`#richiedi-progetto`)
- Occhiello: *Contattaci* — Titolo: **Richiedi un Progetto**
- Testo: *"Raccontaci la tua idea: ti risponderemo entro 48 ore lavorative con una prima valutazione."*

| Campo | Tipo | Obbligatorio | Opzioni |
|---|---|---|---|
| Tipologia di progetto | select | sì | Film / TV · Evento / Allestimento · Restauro · Collezionismo / Privato |
| Nome Azienda / Produzione / Nome e Cognome | testo | sì | — |
| Telefono | tel | sì | — |
| Email | email | sì | — |
| Tempistiche desiderate | select | sì | Urgente (< 2 settimane) · 1 - 3 mesi · In programmazione |
| Budget indicativo | select | sì | < 5.000€ · 5.000€ - 15.000€ · > 15.000€ · Da definire |
| Descrizione del progetto / Note | textarea | sì | — |
| Moodboard / Bozzetti | file (PDF/JPG/PNG, multiplo) | no | — |
| Consenso Privacy Policy | checkbox | sì | — |

- CTA: **Invia Richiesta Progetto**
- Messaggio successo: *"Grazie! La tua richiesta è stata inviata. Il nostro team ti risponderà entro 48 ore lavorative."*
- Messaggio errore: *"Non siamo riusciti a inviare la richiesta. Riprova, oppure scrivici direttamente a info@famigliadeangelis.it."*
- Anti-spam: honeypot nascosto.

### 11. CONTATTI (`#contatti`)
| | |
|---|---|
| Laboratorio | Studi di Cinecittà, Via Tuscolana 1055 — Roma |
| Telefono | +39 06 0000 0000 *(sostituire)* |
| Email | info@famigliadeangelis.it |

### 12. FOOTER
- **FAMIGLIA DE ANGELIS** / CINESCULTURA DAL 1919 / *"Laboratorio di scultura e scenografia cinematografica negli Studi di Cinecittà. Quattro generazioni al servizio del cinema italiano e internazionale."*
- Link Rapidi: Download Media Kit (PDF) · Privacy Policy · Cookie Policy · Credits — *(tutti segnaposto)*
- Seguici: Instagram · TikTok · LinkedIn — *(link segnaposto)*
- Legale: `FAMIGLIA DE ANGELIS S.R.L. — P.IVA 17033161005` · `Sede: Via Libero Leonardi 110 — Laboratori a Cinecittà, Roma` · `© [anno corrente] Famiglia De Angelis S.r.l. Tutti i diritti riservati.`
- Nota crediti: fotografie ed elementi grafici segnaposto da Wikimedia Commons (licenze CC/PD).

---

## 5. Comportamenti JavaScript

| Funzione | Descrizione |
|---|---|
| Header on scroll | ombra dopo 40px di scroll |
| Menu mobile | toggle hamburger; chiusura al click su un link |
| Parallax | foto di sfondo traslate in `translate3d` in base allo scroll, con `requestAnimationFrame` |
| Reveal | `IntersectionObserver` aggiunge `.in-view` agli elementi `.fade-up` (una volta sola) |
| Contatori | animazione ease-out da 0 al `data-target` quando visibili |
| Filtri portfolio | mostra/nasconde `.portfolio-item` per `data-category`; aggiorna lo stato attivo del bottone |
| Etichetta upload | mostra nome file / conteggio |
| Invio modulo | `fetch` POST (FormData) all'endpoint FormSubmit AJAX; disabilita il bottone durante l'invio; mostra riquadro successo o errore; reset dopo 4,5s |
| Statua 3D | init lazy via `IntersectionObserver`; scena Three.js con canvas trasparente, 4 luci (calde + rim dorato + fill freddo), materiale marmoreo (`#EDE3D0`); camera auto-fit sul bounding sphere; `rotation.y = 0.6 + scrollY * 0.0022`; fallback testuale se WebGL/Three non disponibili |
| Anno | `#year` = anno corrente |
| Reduced motion | tutte le animazioni CSS e il parallax vengono disattivati |

---

## 6. File e deploy

```
FamigliaDeAngelisSRL/
├── index.html        # tutto il sito (markup + CSS + JS + modello 3D in base64)
├── README.md         # note rapide
├── BRIEF-SITO.md     # questo documento
└── .gitignore
```

- Push su `main` → Vercel ricostruisce e pubblica automaticamente.
- Nessun comando di build. Vercel serve `index.html` come sito statico.
- Attivazione modulo: al primo invio reale, FormSubmit manda un'email di conferma all'indirizzo destinatario; va cliccato il link una volta.

---

## 7. Cosa è segnaposto (da sostituire prima del lancio definitivo)

- **Tutte le fotografie.** Sono immagini reali ma a licenza libera (Wikimedia Commons, CC/PD), scelte per tema/luogo, **non** foto del laboratorio De Angelis. **Nessuna locandina o fotogramma dei film** (Ben-Hur, Il Gladiatore, ecc.): sono protetti da copyright degli studios e non utilizzabili senza licenza. Vanno rimpiazzate con l'archivio fotografico proprio dell'azienda (foto di scena, backstage, opere realizzate).
- **Statua 3D:** il Discobolo è un segnaposto CC0; se l'azienda ha la scansione 3D di una propria opera, si può sostituire il modello.
- **Contatti reali:** telefono (`+39 06 0000 0000`), eventuale WhatsApp, indirizzo email definitivo.
- **Link footer:** Media Kit PDF, Privacy Policy, Cookie Policy, Credits (oggi puntano a `#`).
- **Social:** URL reali di Instagram, TikTok, LinkedIn.
- **Magazine:** gli articoli sono solo titoli/anteprime, serve contenuto vero o va rimossa/nascosta la sezione.
- **Portfolio:** i 9 progetti hanno descrizioni brevi generiche; vanno verificati con i dati reali dei lavori effettivamente svolti.
- **Logo:** attualmente solo testo; se esiste un logo grafico va inserito.
- **Cookie/Privacy:** se il sito userà analytics o cookie, servono banner di consenso e pagine legali reali (adempimenti GDPR).

---

## 8. Idee / possibili sviluppi futuri

- Self-hosting delle immagini (togliere l'hotlink da Wikimedia) e ottimizzazione (WebP/AVIF, `loading="lazy"`).
- Inlining di font e CSS Tailwind compilato per eliminare le dipendenze CDN a runtime.
- Pagina/PDF "Media Kit" scaricabile.
- Sezione Magazine con articoli reali (anche solo 2–3).
- Automazione: salvataggio locale delle richieste del modulo in una cartella sul computer del titolare.
- Multilingua IT/EN (il target è anche internazionale).
- Micro-CMS o file JSON per gestire portfolio e magazine senza toccare l'HTML.
