<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tutto sul Kernel</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <link rel="stylesheet" href="https://unpkg.com/aos@next/dist/aos.css" />
  <style>
    /* --- Variabili CSS per il Tema --- */
    :root {
      /* Tema Scuro (default) */
      --body-bg: linear-gradient(to right, #1c1c1c, #2e2e2e);
      --text-color: #f5f5f5;
      --header-bg: #111;
      --nav-bg: #0a0a0a;
      --nav-link-color: #ccc;
      --nav-link-hover-bg: #222;
      --nav-link-active-bg: #222;
      --section-bg: linear-gradient(to right, rgba(28,28,28,0.85), rgba(46,46,46,0.85));
      --inner-content-bg: #222;
      --border-color: #333;
      --shadow-color: rgba(0,0,0,0.4);
      --footer-bg: #111;
      --footer-color: #888;
      --highlight-color: #00bcd4; /* Colore principale per accenti e titoli */
      --faq-item-bg: #2a2a2a;
      --faq-answer-bg: #1a1a1a;
      --search-input-bg: #333;
      --search-input-color: #f5f5f5;
      --search-input-placeholder: #aaa;
      --search-results-bg: #1a1a1a;
      --search-results-border: #444;
      --search-results-link-color: #f5f5f5;
      --search-results-link-hover: #00bcd4;
      --toc-bg: #2a2a2a;
      --toc-border: #444;
      --toc-link-color: #ccc;
      --toc-link-hover: #00bcd4;
      --toc-title-color: #00bcd4;
      --code-bg: #1a1a1a;
      --code-color: #f5f5f5;
    }

    body.light-theme {
      /* Tema Chiaro */
      --body-bg: linear-gradient(to right, #f0f0f0, #e0e0e0);
      --text-color: #333;
      --header-bg: #e5e5e5;
      --nav-bg: #d9d9d9;
      --nav-link-color: #555;
      --nav-link-hover-bg: #c0c0c0;
      --nav-link-active-bg: #c0c0c0;
      --section-bg: linear-gradient(to right, rgba(240,240,240,0.85), rgba(224,224,224,0.85));
      --inner-content-bg: #ffffff;
      --border-color: #ccc;
      --shadow-color: rgba(0,0,0,0.15);
      --footer-bg: #e5e5e5;
      --footer-color: #666;
      --highlight-color: #007bb6; /* Un blu più adatto al tema chiaro */
      --faq-item-bg: #f8f8f8;
      --faq-answer-bg: #ffffff;
      --search-input-bg: #eee;
      --search-input-color: #333;
      --search-input-placeholder: #888;
      --search-results-bg: #f5f5f5;
      --search-results-border: #bbb;
      --search-results-link-color: #333;
      --search-results-link-hover: #007bb6;
      --toc-bg: #e0e0e0;
      --toc-border: #ccc;
      --toc-link-color: #555;
      --toc-link-hover: #007bb6;
      --toc-title-color: #007bb6;
      --code-bg: #f0f0f0;
      --code-color: #333;
    }

    /* --- Stili Base --- */
    body {
      margin: 0;
      font-family: 'Roboto', sans-serif;
      background: var(--body-bg);
      color: var(--text-color);
      line-height: 1.6;
      height: 100vh;
      display: flex;
      flex-direction: column;
      overflow: hidden; /* Nasconde le scrollbar extra sul body */
      position: relative;
      transition: background 0.5s ease, color 0.5s ease; /* Transizione fluida per il tema */
    }

    /* --- Pre-loader --- */
    #loader {
      position: fixed;
      left: 50%;
      top: 50%;
      z-index: 1000;
      border: 8px solid var(--border-color);
      border-top: 8px solid var(--highlight-color);
      border-radius: 50%;
      width: 60px;
      height: 60px;
      animation: spin 1s linear infinite;
      display: none; /* Nascosto di default, mostrato da JS */
      transform: translate(-50%, -50%);
    }

    @keyframes spin {
      0% { transform: translate(-50%, -50%) rotate(0deg); }
      100% { transform: translate(-50%, -50%) rotate(360deg); }
    }

    /* --- Canvas per lo sfondo animato --- */
    #particles-js {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -2;
      background: transparent;
    }

    /* --- Elementi grafici sui lati (pseudo-elementi) --- */
    body::before, body::after {
      content: '';
      position: absolute;
      top: 0;
      width: 20%;
      height: 100%;
      background-image: url('https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Circuit_Board_Vector.svg/1280px-Circuit_Board_Vector.svg.png');
      background-size: cover;
      background-repeat: no-repeat;
      opacity: 0.03; /* Leggermente visibili */
      z-index: -1;
      pointer-events: none; /* Non interferiscono con il mouse */
    }

    body::before {
      left: 0;
      transform: scaleX(-1); /* Specchia l'immagine per il lato sinistro */
    }

    body::after {
      right: 0;
    }

    /* --- Header --- */
    header {
      background: var(--header-bg);
      padding: 20px;
      display: flex;
      flex-direction: column; /* Colonna per titolo, ricerca e tema */
      align-items: center;
      justify-content: center;
      z-index: 100;
      box-shadow: 0 2px 10px var(--shadow-color);
      flex-shrink: 0; /* Impedisce che l'header si riduca */
      transition: background 0.5s ease;
    }

    header h1 {
      margin: 0;
      font-size: 1.8em;
      color: var(--highlight-color);
      text-align: center;
      margin-bottom: 15px; /* Spazio sotto il titolo */
      transition: color 0.5s ease;
    }

    .header-controls {
        display: flex;
        align-items: center;
        gap: 15px; /* Spazio tra gli elementi */
    }

    /* --- Barra di Ricerca --- */
    #search-container {
        position: relative;
    }

    #search-input {
        padding: 10px 15px;
        border: 1px solid var(--border-color);
        border-radius: 20px;
        background-color: var(--search-input-bg);
        color: var(--search-input-color);
        font-size: 1em;
        width: 250px;
        transition: all 0.3s ease;
    }

    #search-input::placeholder {
        color: var(--search-input-placeholder);
    }

    #search-input:focus {
        outline: none;
        border-color: var(--highlight-color);
        box-shadow: 0 0 8px rgba(0, 188, 212, 0.4);
    }

    /* --- Risultati della Ricerca --- */
    #search-results-list {
        position: absolute;
        top: 100%; /* Sotto l'input */
        left: 0;
        width: 100%;
        background-color: var(--search-results-bg);
        border: 1px solid var(--search-results-border);
        border-radius: 5px;
        box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        max-height: 200px;
        overflow-y: auto;
        list-style: none;
        padding: 0;
        margin-top: 5px;
        z-index: 10; /* Sopra gli altri elementi */
        display: none; /* Nascosto di default */
    }

    #search-results-list li {
        padding: 10px 15px;
        border-bottom: 1px solid var(--search-results-border);
        cursor: pointer;
        transition: background-color 0.3s ease;
    }

    #search-results-list li:last-child {
        border-bottom: none;
    }

    #search-results-list li:hover {
        background-color: var(--nav-link-hover-bg);
    }

    #search-results-list li a {
        color: var(--search-results-link-color);
        text-decoration: none;
        display: block;
        transition: color 0.3s ease;
    }

    #search-results-list li a:hover {
        color: var(--search-results-link-hover);
    }

    /* --- Theme Switcher --- */
    #theme-switcher {
      background: var(--search-input-bg);
      border: 1px solid var(--border-color);
      border-radius: 50%;
      width: 40px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      color: var(--highlight-color);
      font-size: 1.2em;
      transition: all 0.3s ease;
    }

    #theme-switcher:hover {
      background-color: var(--nav-link-hover-bg);
      transform: scale(1.1);
    }
    #theme-switcher i {
        pointer-events: none; /* Impedisce che il click sia catturato dall'icona */
    }

    /* --- Navigazione Principale --- */
    nav.top-nav {
      background: var(--nav-bg);
      text-align: center;
      padding: 10px 0;
      z-index: 90;
      box-shadow: 0 2px 5px var(--shadow-color);
      flex-shrink: 0;
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      transition: background 0.5s ease;
    }

    nav.top-nav a {
      display: flex;
      align-items: center;
      padding: 10px 20px;
      color: var(--nav-link-color);
      text-decoration: none;
      transition: background 0.3s ease, color 0.3s ease, transform 0.2s ease;
      white-space: nowrap; /* Evita che i link vadano a capo */
      margin: 0 5px;
    }

    nav.top-nav a i {
      margin-right: 8px;
    }

    nav.top-nav a:hover {
      background: var(--nav-link-hover-bg);
      color: var(--highlight-color);
      border-radius: 5px;
      transform: scale(1.05);
    }

    nav.top-nav a.active {
      color: var(--highlight-color);
      font-weight: bold;
      background: var(--nav-link-active-bg);
      border-radius: 5px;
    }

    /* --- Sezioni Contenuto Principali --- */
    main {
      flex-grow: 1;
      position: relative;
      overflow: hidden; /* Importante per le transizioni delle sezioni */
    }

    section.content-page {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start; /* Allinea il contenuto in alto */
      background: var(--section-bg);
      opacity: 0;
      pointer-events: none; /* Non cliccabile se non attiva */
      transform: translateX(100%); /* Parte da destra */
      transition: opacity 0.7s ease-in-out, transform 0.7s ease-in-out, background 0.5s ease;
      box-sizing: border-box;
      padding: 20px;
      overflow-y: auto; /* Abilita lo scroll per il contenuto lungo */
      -webkit-overflow-scrolling: touch; /* Migliora lo scroll su iOS */
    }

    section.content-page.active {
      opacity: 1;
      pointer-events: auto;
      transform: translateX(0);
      z-index: 1;
    }

    section.content-page.leaving {
      transform: translateX(-100%); /* Esce a sinistra */
      opacity: 0;
      z-index: 0;
    }

    section .inner-content {
      max-width: 900px;
      width: 100%;
      padding: 30px;
      background: var(--inner-content-bg);
      border-radius: 10px;
      box-shadow: 0 6px 20px var(--shadow-color);
      line-height: 1.8;
      border: 1px solid var(--border-color);
      text-align: left;
      box-sizing: border-box;
      transition: background 0.5s ease, border-color 0.5s ease, box-shadow 0.5s ease;
    }

    section h2 {
      color: var(--highlight-color);
      border-bottom: 2px solid var(--highlight-color);
      padding-bottom: 10px;
      margin-bottom: 25px;
      font-size: 2.2em;
      transition: color 0.5s ease, border-color 0.5s ease;
    }

    section h3 {
      color: var(--highlight-color);
      margin-top: 30px;
      margin-bottom: 15px;
      font-size: 1.6em;
      transition: color 0.5s ease;
    }

    section p {
      margin-bottom: 15px;
      text-align: justify;
    }

    section ul {
      list-style-type: disc;
      margin-left: 25px;
      margin-bottom: 15px;
    }

    section ul li {
      margin-bottom: 8px;
    }

    .section-image {
      display: block;
      max-width: 100%;
      height: auto;
      margin: 20px auto;
      border-radius: 8px;
      box-shadow: 0 4px 10px var(--shadow-color);
    }

    /* Stili per blocchi di codice (opzionale se non usi Markdown per il codice) */
    code {
        font-family: monospace;
        background-color: var(--code-bg);
        color: var(--code-color);
        padding: 2px 4px;
        border-radius: 4px;
    }

    /* --- Sezione FAQ (Accordion) --- */
    .faq-item {
      background-color: var(--faq-item-bg);
      border: 1px solid var(--border-color);
      border-radius: 8px;
      margin-bottom: 15px;
      overflow: hidden;
      transition: background-color 0.5s ease, border-color 0.5s ease;
    }

    .faq-question {
      padding: 20px;
      cursor: pointer;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-weight: bold;
      color: var(--highlight-color);
      font-size: 1.1em;
      transition: background-color 0.3s ease, color 0.5s ease;
    }

    .faq-question:hover {
      background-color: var(--nav-link-hover-bg);
    }

    .faq-question i {
      transition: transform 0.3s ease;
    }

    .faq-question.active i {
      transform: rotate(180deg);
    }

    .faq-answer {
      padding: 0 20px;
      background-color: var(--faq-answer-bg);
      max-height: 0; /* Inizialmente chiuso */
      overflow: hidden;
      transition: max-height 0.4s ease-out, padding 0.4s ease-out, background-color 0.5s ease;
      color: var(--text-color);
    }

    .faq-answer p {
      padding-top: 15px;
      padding-bottom: 15px;
      margin: 0;
    }

    /* --- Sezione Glossario --- */
    .glossary-term {
      margin-bottom: 25px;
    }

    .glossary-term h3 {
      color: var(--highlight-color);
      margin-bottom: 10px;
      font-size: 1.5em;
      transition: color 0.5s ease;
    }

    .glossary-term p {
      margin-bottom: 5px;
      text-align: justify;
    }

    /* --- Evidenziazione della Ricerca --- */
    .highlight {
        background-color: yellow;
        color: black;
        padding: 2px 0;
        border-radius: 3px;
        display: inline; /* Importante per non rompere il layout dei paragrafi */
        line-height: 1; /* A volte utile per evitare extra spaziatura */
    }

    /* --- Sommario Dinamico (Table of Contents) --- */
    .table-of-contents {
        background-color: var(--toc-bg);
        border: 1px solid var(--toc-border);
        border-radius: 8px;
        padding: 20px;
        margin-bottom: 30px;
        box-shadow: 0 2px 10px var(--shadow-color);
        transition: background-color 0.5s ease, border-color 0.5s ease, box-shadow 0.5s ease;
    }

    .table-of-contents h3 {
        color: var(--toc-title-color);
        margin-top: 0;
        margin-bottom: 15px;
        font-size: 1.3em;
        border-bottom: 1px solid var(--toc-border);
        padding-bottom: 10px;
        transition: color 0.5s ease, border-color 0.5s ease;
    }

    .table-of-contents ul {
        list-style: none;
        padding: 0;
        margin: 0;
    }

    .table-of-contents ul li {
        margin-bottom: 10px;
    }

    .table-of-contents ul li a {
        color: var(--toc-link-color);
        text-decoration: none;
        transition: color 0.3s ease;
        display: block;
        padding: 5px 0;
    }

    .table-of-contents ul li a:hover {
        color: var(--toc-link-hover);
        text-decoration: underline;
    }

    /* --- Footer --- */
    footer {
      text-align: center;
      padding: 10px;
      background: var(--footer-bg);
      color: var(--footer-color);
      box-shadow: 0 -2px 10px var(--shadow-color);
      z-index: 100;
      flex-shrink: 0;
      font-size: 0.8em;
      transition: background 0.5s ease, color 0.5s ease, box-shadow 0.5s ease;
    }

    /* --- Pulsante "Torna su" --- */
    #backToTopBtn {
      display: none; /* Nascosto di default */
      position: fixed;
      bottom: 50px;
      right: 50px;
      z-index: 999;
      border: none;
      outline: none;
      background-color: var(--highlight-color);
      color: white;
      cursor: pointer;
      padding: 18px;
      border-radius: 50%;
      font-size: 1.3em;
      box-shadow: 0 6px 12px var(--shadow-color);
      opacity: 0; /* Inizialmente trasparente */
      transform: translateY(20px); /* Leggermente spostato per l'animazione */
      transition: all 0.4s ease-out; /* Transizione fluida */
    }

    #backToTopBtn.show {
      opacity: 1;
      transform: translateY(0);
    }

    #backToTopBtn:hover {
      background-color: #0097a7; /* Colore leggermente più scuro all'hover */
      transform: scale(1.1);
    }

    /* --- Media Queries per la Reattività --- */
    @media (max-width: 1024px) {
        body::before, body::after {
            display: none; /* Nasconde gli elementi grafici sui lati su schermi più piccoli */
        }
        .header-controls {
            flex-direction: column; /* Impila gli elementi dell'header in colonna */
            gap: 10px;
        }
        #search-input {
            width: 80vw; /* Più largo su schermi piccoli */
        }
        #search-results-list {
            width: calc(80vw + 30px); /* Regola la larghezza per mobile */
        }
    }

    @media (max-width: 767px) {
      nav.top-nav {
        padding: 5px 0;
      }
      nav.top-nav a {
        display: block; /* Ogni link occupa una riga */
        border-bottom: 1px solid var(--border-color);
        padding: 8px 15px;
        margin: 0;
      }
      nav.top-nav a:last-child {
        border-bottom: none; /* Rimuove il bordo inferiore dall'ultimo link */
      }
      header {
        padding: 15px;
      }
      header h1 {
        font-size: 1.5em;
      }
      section.content-page {
        padding: 10px;
      }
      section .inner-content {
        margin: 10px;
        padding: 20px;
      }
      section h2 {
        font-size: 1.8em;
      }
      footer {
        padding: 8px;
        font-size: 0.7em;
      }
      #backToTopBtn {
        bottom: 30px;
        right: 30px;
        padding: 15px;
        font-size: 1.1em;
      }
    }
  </style>
</head>
<body>
  <div id="loader"></div>

  <canvas id="particles-js"></canvas>

  <header>
    <h1>Il Kernel del Sistema Operativo</h1>
    <div class="header-controls">
        <div id="search-container">
            <input type="search" id="search-input" placeholder="Cerca nel sito..." aria-label="Cerca nel sito">
            <ul id="search-results-list"></ul> </div>
        <button id="theme-switcher" aria-label="Cambia tema">
            <i class="fas fa-sun"></i> </button>
    </div>
  </header>

  <nav class="top-nav">
    <a href="#home" class="active"><i class="fas fa-home"></i>Home</a>
    <a href="#cos-e"><i class="fas fa-question-circle"></i>Cos'è</a>
    <a href="#tipi"><i class="fas fa-project-diagram"></i>Tipi</a>
    <a href="#funzioni"><i class="fas fa-cogs"></i>Funzioni</a>
    <a href="#esempi"><i class="fas fa-laptop-code"></i>Esempi</a>
    <a href="#glossario"><i class="fas fa-book"></i>Glossario</a>
    <a href="#faq"><i class="fas fa-comments"></i>FAQ</a>
    <a href="#curiosita"><i class="fas fa-lightbulb"></i>Curiosità</a>
  </nav>

  <main>
    <section id="home" class="content-page active">
      <div class="inner-content" data-aos="fade-up">
        <h2>Benvenuto nel Mondo del Kernel!</h2>
        <p>Esplora il cuore pulsante di ogni sistema operativo. Il **kernel** è il componente più fondamentale di un sistema operativo, responsabile della gestione delle risorse hardware e del software. Senza di esso, nessun programma potrebbe funzionare.</p>
        <p>Questo sito ti guiderà attraverso le sue definizioni, i vari tipi esistenti, le sue funzioni vitali, esempi pratici e alcune curiosità affascinanti. Preparati a scoprire come il tuo computer prende vita!</p>
        <img src="https://picsum.photos/seed/kernel1/800/400" alt="Diagramma Kernel - Componenti base di un sistema operativo" class="section-image">
        <p>Il kernel agisce come un ponte tra le applicazioni e l'hardware del sistema, permettendo al software di richiedere e utilizzare le risorse fisiche come la CPU, la memoria, il disco e i dispositivi di input/output. È una componente complessa ma elegantemente progettata che garantisce la stabilità, la sicurezza e le prestazioni del tuo dispositivo.</p>
      </div>
    </section>

    <section id="cos-e" class="content-page">
      <div class="inner-content" data-aos="fade-up">
        <h2>Cos'è il Kernel?</h2>
        <p>Il **kernel** è il cuore, il nucleo fondamentale di ogni sistema operativo. È il primo programma a essere caricato all'avvio del computer, ancor prima che tu veda la schermata di login. Una volta caricato, rimane residente in memoria per tutta la durata del funzionamento del sistema, gestendo tutte le operazioni essenziali.</p>

        <h3>Il Ruolo Centrale</h3>
        <p>Immagina il kernel come il direttore d'orchestra del tuo computer. Ogni volta che un'applicazione ha bisogno di fare qualcosa di "importante" — come leggere un file dal disco, inviare dati sulla rete o addirittura stampare un documento — non lo fa direttamente. Invece, l'applicazione invia una **chiamata di sistema** al kernel.</p>
        <p>Il kernel, con i suoi privilegi elevati (modalità kernel o modalità supervisore), esegue l'operazione richiesta in modo sicuro e controllato. Questo è cruciale per la stabilità e la sicurezza del sistema: se ogni applicazione potesse accedere direttamente all'hardware, il caos sarebbe assicurato, e un'applicazione malfunzionante potrebbe compromettere l'intero sistema.</p>
        <img src="https://picsum.photos/seed/kernel2/800/400" alt="Processore e memoria in un sistema operativo" class="section-image">

        <h3>Componenti Chiave</h3>
        <p>Ecco i componenti chiave del kernel:</p>
        <ul>
          <li>**Gestore dei processi:** Controlla l'esecuzione dei programmi, decidendo quale processo può usare la CPU e per quanto tempo.</li>
          <li>**Gestore della memoria:** Alloca e dealloca memoria RAM ai processi, evitando che un programma invada lo spazio di un altro.</li>
          <li>**Gestore dei dispositivi (I/O):** Interagisce con l'hardware come dischi rigidi, stampanti, tastiere e mouse attraverso i driver.</li>
          <li>**Sistema di gestione dei file:** Organizza e gestisce i file e le directory su dispositivi di archiviazione.</li>
          <li>**Comunicazione interprocesso (IPC):** Permette ai diversi programmi di scambiare dati e informazioni tra loro.</li>
        </ul>
        <p>In sintesi, il kernel fornisce i servizi fondamentali che consentono ai programmi di funzionare e interagire con l'hardware, mantenendo l'ordine e la sicurezza dell'intero sistema.</p>
      </div>
    </section>

    <section id="tipi" class="content-page">
      <div class="inner-content" data-aos="fade-up">
        <h2>Tipi di Kernel</h2>
        <p>Esistono diverse architetture di kernel, ciascuna con i propri vantaggi e svantaggi in termini di prestazioni, modularità e robustezza. Le tre categorie principali sono monolitici, microkernel e ibridi.</p>

        <h3>1. Kernel Monolitico</h3>
        <p>Il kernel monolitico è l'architettura più tradizionale. In questo modello, tutte le funzionalità del sistema operativo (gestione dei processi, della memoria, dei file system, dei driver di dispositivo, ecc.) sono contenute in un unico grande blocco di codice eseguito nello spazio del kernel (modalità privilegiata).</p>
        <p>Ecco i suoi punti di forza e debolezza:</p>
        <ul>
          <li>**Vantaggi:**
            <ul>
              <li>**Velocità:** La comunicazione tra i vari componenti è molto rapida poiché risiedono tutti nello stesso spazio di indirizzamento.</li>
              <li>**Sviluppo più semplice:** È più semplice da implementare inizialmente rispetto a un microkernel.</li>
            </ul>
          </li>
          <li>**Svantaggi:**
            <ul>
              <li>**Meno robusto:** Un errore in un driver o in un modulo può potenzialmente far crashare l'intero sistema.</li>
              <li>**Difficile da estendere:** Aggiungere nuove funzionalità o rimuoverne di vecchie richiede spesso la ricompilazione dell'intero kernel.</li>
              <li>**Grande dimensione:** Il codice del kernel può diventare molto grande e complesso.</li>
            </ul>
          </li>
        </ul>
        <img src="https://picsum.photos/seed/kernel3/800/400" alt="Diagramma di un Kernel Monolitico" class="section-image">
        <p>Alcuni esempi noti includono: **Linux**, le prime versioni di Unix, MS-DOS.</p>

        <h3>2. Microkernel</h3>
        <p>A differenza del monolitico, un microkernel riduce al minimo le funzionalità eseguite nello spazio del kernel. Solo le funzioni essenziali come la gestione della comunicazione interprocesso (IPC), la gestione della memoria di basso livello e la pianificazione dei processi vengono mantenute nel kernel.</p>
        <p>Altre funzionalità come i driver di dispositivo, i file system, la gestione della rete e i gestori dei processi vengono implementate come processi utente separati, chiamati **server**.</p>
        <p>Vediamo i pro e i contro:</p>
        <ul>
          <li>**Vantaggi:**
            <ul>
              <li>**Robustezza:** Un errore in un server (es. un driver) non fa crashare l'intero kernel, ma solo quel server specifico.</li>
              <li>**Modularità:** È facile aggiungere o rimuovere funzionalità senza dover ricompilare il kernel.</li>
              <li>**Sicurezza:** Meno codice nel kernel significa una superficie di attacco ridotta.</li>
            </ul>
          </li>
          <li>**Svantaggi:**
            <ul>
              <li>**Prestazioni:** Le chiamate tra server e kernel sono più lente a causa dei cambi di contesto e delle comunicazioni interprocesso.</li>
              <li>**Complessità di sviluppo:** Progettare e implementare un microkernel efficiente è molto più difficile.</li>
            </ul>
          </li>
        </ul>
        <img src="https://picsum.photos/seed/kernel4/800/400" alt="Diagramma di un Microkernel" class="section-image">
        <p>Esempi di microkernel includono: **Minix**, GNU Hurd, QNX (usato in sistemi embedded critici come le auto).</p>

        <h3>3. Kernel Ibrido (o Modulare)</h3>
        <p>Il kernel ibrido tenta di combinare i vantaggi sia dei kernel monolitici che dei microkernel. Tipicamente, la maggior parte dei servizi è integrata nel kernel (come in un monolitico), ma alcuni componenti, come i driver di dispositivo, possono essere caricati e scaricati dinamicamente come moduli.</p>
        <p>Questo permette un buon equilibrio tra prestazioni e modularità, offrendo la velocità dei monolitici e parte della flessibilità dei microkernel.</p>
        <ul>
          <li>**Vantaggi:**
            <ul>
              <li>**Equilibrio:** Un buon compromesso tra prestazioni e modularità/robustezza.</li>
              <li>**Flessibilità:** Possibilità di caricare moduli al bisogno senza riavviare il sistema.</li>
            </ul>
          </li>
          <li>**Svantaggi:**
            <ul>
              <li>Possono ancora essere soggetti a crash se un modulo caricato ha un grave difetto.</li>
            </ul>
          </li>
        </ul>
        <img src="https://picsum.photos/seed/kernel5/800/400" alt="Diagramma di un Kernel Ibrido" class="section-image">
        <p>Esempi comuni sono: **Windows NT** (e tutte le versioni moderne di Windows), **XNU** (il kernel di macOS, iOS, iPadOS), alcune implementazioni di BSD.</p>
      </div>
    </section>

    <section id="funzioni" class="content-page">
      <div class="inner-content" data-aos="fade-up">
        <h2>Funzioni Principali del Kernel</h2>
        <p>Il kernel è il motore che fa funzionare ogni aspetto del sistema operativo. Le sue funzioni sono critiche e interconnesse, garantendo che hardware e software lavorino in armonia.</p>

        <h3>1. Gestione dei Processi (Process Management)</h3>
        <p>Questa è forse la funzione più visibile e dinamica del kernel. Un **processo** è un'istanza di un programma in esecuzione. Il kernel è responsabile di:</p>
        <ul>
          <li>**Creazione e Terminazione:** Avvia nuovi processi quando apri un'applicazione e li termina quando la chiudi.</li>
          <li>**Pianificazione (Scheduling):** Decide quale processo può usare la CPU in un dato momento. Poiché la CPU può eseguire un solo processo alla volta (per core), il kernel passa rapidamente tra i processi attivi (<em>context switching</em>) per dare l'illusione che stiano tutti eseguendo contemporaneamente.</li>
          <li>**Comunicazione Interprocesso (IPC):** Fornisce meccanismi per i processi di comunicare e scambiare dati tra loro in modo sicuro (es. pipe, socket, shared memory).</li>
          <li>**Sincronizzazione:** Gestisce l'accesso alle risorse condivise per evitare conflitti tra i processi.</li>
        </ul>
        <img src="https://picsum.photos/seed/function1/800/400" alt="Icona di multitasking e gestione processi" class="section-image">

        <h3>2. Gestione della Memoria (Memory Management)</h3>
        <p>La memoria RAM è una risorsa finita e cruciale. Il kernel la gestisce per garantire che ogni processo abbia lo spazio necessario senza interferire con gli altri.</p>
        <ul>
          <li>**Allocazione e Deallocazione:** Assegna blocchi di memoria ai processi quando ne hanno bisogno e li recupera quando non servono più.</li>
          <li>**Memoria Virtuale:** Consente ai processi di accedere a uno spazio di indirizzamento virtuale che è mappato sulla memoria fisica (RAM) o su disco (memoria di swap). Questo permette ai processi di usare più memoria di quanta ne sia fisicamente disponibile e isola i processi l'uno dall'altro.</li>
          <li>**Protezione della Memoria:** Impedisce a un processo di accedere allo spazio di memoria di un altro processo o del kernel stesso, prevenendo crash e violazioni della sicurezza.</li>
        </ul>
        <img src="https://picsum.photos/seed/function2/800/400" alt="Immagine che rappresenta la gestione della memoria" class="section-image">

        <h3>3. Gestione I/O (Input/Output Management)</h3>
        <p>L'interazione con i dispositivi hardware (tastiera, mouse, stampante, disco rigido, schede di rete, ecc.) è mediata dal kernel.</p>
        <ul>
          <li>**Driver di Dispositivo:** Il kernel carica e gestisce i driver, che sono programmi specifici per comunicare con un particolare hardware.</li>
          <li>**Buffer e Cache:** Utilizza buffer per gestire i dati in entrata e in uscita dai dispositivi, e cache per migliorare le prestazioni.</li>
          <li>**Polling e Interrupts:** Gestisce come il kernel e i dispositivi comunicano, sia controllando periodicamente i dispositivi (polling) sia ricevendo segnali dai dispositivi quando hanno completato un'operazione (interrupts).</li>
        </ul>
        <img src="https://picsum.photos/seed/function3/800/400" alt="Icona di periferiche e input/output" class="section-image">

        <h3>4. Gestione del File System</h3>
        <p>Il kernel gestisce l'organizzazione, l'archiviazione e l'accesso ai file su dispositivi di archiviazione come dischi rigidi e SSD.</p>
        <ul>
          <li>**Struttura delle Directory:** Mantiene la gerarchia di file e cartelle.</li>
          <li>**Allocazione dello Spazio:** Decide dove i file vengono memorizzati sul disco e come lo spazio viene recuperato.</li>
          <li>**Permessi:** Applica le regole di accesso ai file e alle directory, garantendo che solo gli utenti autorizzati possano leggere, scrivere o eseguire determinati file.</li>
        </ul>
        <img src="https://picsum.photos/seed/function4/800/400" alt="Icona di gestione file e cartelle" class="section-image">

        <h3>5. Gestione della Sicurezza</h3>
        <p>La sicurezza è intrinsecamente legata alle funzioni del kernel, poiché controlla l'accesso alle risorse critiche.</p>
        <ul>
          <li>**Modalità Operative:** Il kernel esegue in modalità privilegiata (kernel mode), mentre le applicazioni utente eseguono in modalità utente (user mode), con accesso limitato alle risorse hardware e di memoria.</li>
          <li>**Controllo degli Accessi:** Applica le politiche di sicurezza per l'accesso a file, processi e risorse hardware.</li>
        </ul>
        <img src="https://picsum.photos/seed/function5/800/400" alt="Icona di sicurezza informatica" class="section-image">

        <h3>6. Chiamate di Sistema (System Calls)</h3>
        <p>Le chiamate di sistema sono l'interfaccia principale tra le applicazioni utente e il kernel. Quando un programma ha bisogno di eseguire un'operazione privilegiata (es. aprire un file, creare un processo, accedere alla rete), effettua una chiamata di sistema. Il kernel intercetta questa richiesta, la valida e la esegue, restituendo il risultato all'applicazione.</p>
        <p>Queste funzioni lavorano insieme per fornire l'ambiente stabile e funzionale su cui operano tutte le applicazioni che usiamo quotidianamente.</p>
      </div>
    </section>

    <section id="esempi" class="content-page">
      <div class="inner-content" data-aos="fade-up">
        <h2>Esempi di Kernel Famosi</h2>
        <p>Il mondo dei sistemi operativi è vasto e diversificato, e al suo cuore si trovano kernel con architetture e filosofie di sviluppo diverse. Ecco alcuni degli esempi più influenti:</p>

        <h3>1. Linux Kernel</h3>
        <p>Il **Linux kernel** è un kernel monolitico open-source, probabilmente il più diffuso al mondo. Creato da Linus Torvalds nel 1991 come un progetto hobby, è cresciuto fino a diventare la spina dorsale di innumerevoli sistemi.</p>
        <ul>
          <li>**Caratteristiche:** Flessibile, modulare (anche se monolitico, supporta moduli caricabili dinamicamente), altamente performante e incredibilmente scalabile.</li>
          <li>**Dove lo trovi:**
            <ul>
              <li>**Sistemi Operativi:** È il kernel alla base di tutte le distribuzioni Linux (Ubuntu, Fedora, Debian, Mint, Red Hat, SUSE, ecc.).</li>
              <li>**Android:** Ogni smartphone Android è basato sul kernel Linux.</li>
              <li>**Server:** La stragrande maggioranza dei server web, dei server cloud e dei supercomputer di tutto il mondo.</li>
              <li>**Dispositivi Embedded:** Molti router, smart TV, sistemi di infotainment per auto e altri dispositivi IoT.</li>
            </ul>
          </li>
        </ul>
        <img src="https://picsum.photos/seed/linux/800/400" alt="Logo Linux" class="section-image">

        <h3>2. NT Kernel (Windows NT, 2000, XP, Vista, 7, 8, 10, 11)</h3>
        <p>Il **NT kernel** è il cuore dei moderni sistemi operativi Windows, a partire da Windows NT (1993). È un esempio di kernel ibrido.</p>
        <ul>
          <li>**Caratteristiche:** Progettato per la stabilità, la sicurezza e la scalabilità, supporta il multiprocessing simmetrico (SMP) e l'esecuzione su più architetture hardware.</li>
          <li>**Architettura Ibrida:** Alcuni servizi chiave (es. gestione I/O, memoria) sono nel kernel, mentre altri (es. driver di dispositivo) possono essere eseguiti in modalità utente o essere caricati/scaricati dinamicamente.</li>
          <li>**Dove lo trovi:** Ogni installazione di Windows (desktop, server, IoT Core).</li>
        </ul>
        <img src="https://picsum.photos/seed/windows/800/400" alt="Logo Windows" class="section-image">

        <h3>3. XNU (macOS, iOS, iPadOS, watchOS, tvOS)</h3>
        <p>**XNU** è il kernel di macOS (ex OS X) e di tutti i sistemi operativi mobili di Apple. È un kernel ibrido, il cui nome significa "**X** is **N**ot **U**nix".</p>
        <ul>
          <li>**Caratteristiche:** Combina il microkernel **Mach** (per le funzionalità di basso livello come IPC e scheduling) con componenti di **FreeBSD** (per le funzionalità di livello superiore come la gestione dei file system e del networking).</li>
          <li>**Flessibilità:** L'approccio ibrido offre robustezza e prestazioni, permettendo ad Apple di evolvere rapidamente i propri sistemi.</li>
          <li>**Dove lo trovi:** Tutti i dispositivi Apple (Mac, iPhone, iPad, Apple Watch, Apple TV).</li>
        </ul>
        <img src="https://picsum.photos/seed/apple/800/400" alt="Logo Apple" class="section-image">

        <h3>4. Minix</h3>
        <p>**Minix** è un kernel microkernel creato da Andrew S. Tanenbaum nel 1987. È stato progettato principalmente per scopi educativi, per insegnare i principi dei sistemi operativi.</p>
        <ul>
          <li>**Caratteristiche:** Molto piccolo e compatto. La sua architettura è pensata per la massima modularità e robustezza, con pochissimo codice eseguito in modalità kernel.</li>
          <li>**Impatto Storico:** Nonostante non sia ampiamente usato come sistema operativo desktop, Minix ha avuto un'influenza enorme. È stato l'ispirazione diretta per Linus Torvalds, che ha iniziato a sviluppare Linux dopo essere rimasto insoddisfatto delle restrizioni di licenza e del design di Minix.</li>
          <li>**Curiosità:** Per un periodo, si è speculato che Minix fosse usato in alcuni chip Intel Management Engine (ME), una funzionalità che controlla i sistemi Intel, sebbene questo sia un argomento di dibattito e sicurezza.</li>
        </ul>
        <img src="https://picsum.photos/seed/minix/800/400" alt="Illustrazione Minix" class="section-image">

        <p>Questi esempi dimostrano la diversità e l'evoluzione delle architetture kernel, ognuno ottimizzato per specifiche esigenze e ambienti, dai supercomputer ai dispositivi mobili.</p>
      </div>
    </section>

    <section id="glossario" class="content-page">
        <div class="inner-content" data-aos="fade-up">
            <h2>Glossario dei Termini</h2>
            <p>Ecco una lista di termini chiave relativi al kernel e ai sistemi operativi per aiutarti a navigare meglio in questo mondo complesso.</p>

            <div class="glossary-term">
                <h3>Chiamata di Sistema (System Call)</h3>
                <p>È una richiesta programmatica che un programma utente effettua al kernel del sistema operativo per accedere a servizi o risorse privilegiate. Esempi includono la lettura/scrittura di file, la creazione di processi o l'accesso alla rete.</p>
            </div>

            <div class="glossary-term">
                <h3>Modalità Kernel (Kernel Mode / Supervisor Mode)</h3>
                <p>La modalità operativa più privilegiata della CPU, in cui il kernel ha pieno accesso a tutte le risorse hardware e può eseguire qualsiasi istruzione. Un errore in questa modalità può causare un "kernel panic" o un crash del sistema.</p>
            </div>

            <div class="glossary-term">
                <h3>Modalità Utente (User Mode)</h3>
                <p>La modalità operativa meno privilegiata della CPU, in cui le applicazioni utente vengono eseguite. I programmi in modalità utente hanno accesso limitato alle risorse hardware e devono effettuare chiamate di sistema per richiedere servizi al kernel.</p>
            </div>

            <div class="glossary-term">
                <h3>Driver di Dispositivo</h3>
                <p>Un programma software che consente al sistema operativo di comunicare con un particolare dispositivo hardware (es. stampante, scheda grafica, mouse, tastiera). I driver traducono le istruzioni generiche del sistema operativo in comandi specifici che l'hardware può comprendere.</p>
            </div>

            <div class="glossary-term">
                <h3>Interrupt</h3>
                <p>Un segnale inviato da un dispositivo hardware o da un evento software al processore per indicare che un evento che richiede attenzione è avvenuto. Il kernel interrompe temporaneamente il processo corrente per gestire l'interrupt.</p>
            </div>

            <div class="glossary-term">
                <h3>Context Switching (Cambio di Contesto)</h3>
                <p>Il processo attraverso il quale il kernel salva lo stato di un processo (il suo "contesto") e carica lo stato di un altro processo, consentendo alla CPU di passare dall'esecuzione di un processo all'altro. È fondamentale per il multitasking.</p>
            </div>

            <div class="glossary-term">
                <h3>Processo</h3>
                <p>Un'istanza di un programma in esecuzione. Ogni processo ha il proprio spazio di indirizzamento, registri della CPU, stack e risorse allocate dal kernel.</p>
            </div>

            <div class="glossary-term">
                <h3>Thread</h3>
                <p>Una sequenza di istruzioni eseguibili all'interno di un processo. Un processo può avere più thread che condividono lo stesso spazio di memoria e risorse, consentendo l'esecuzione parallela di parti di un programma.</p>
            </div>

            <div class="glossary-term">
                <h3>Memoria Virtuale</h3>
                <p>Una tecnica di gestione della memoria che permette a un programma di avere l'impressione di disporre di una memoria contigua più grande di quella fisica disponibile. Il kernel gestisce la mappatura tra indirizzi virtuali e fisici, utilizzando il disco (swapping) per compensare la mancanza di RAM.</p>
            </div>

            <div class="glossary-term">
                <h3>Filesystem (Sistema di Gestione File)</h3>
                <p>Il metodo e la struttura dati che un sistema operativo utilizza per controllare come i dati sono memorizzati e recuperati su un dispositivo di archiviazione. Organizza i file in un'architettura gerarchica (directory e sottodirectory).</p>
            </div>

            <div class="glossary-term">
                <h3>Kernel Panic</h3>
                <p>Un messaggio di errore critico che si verifica quando il kernel di un sistema operativo rileva un errore interno irrecuperabile da cui non può riprendersi. Porta al blocco completo del sistema e spesso richiede un riavvio.</p>
            </div>
        </div>
    </section>

    <section id="faq" class="content-page">
        <div class="inner-content" data-aos="fade-up">
            <h2>Domande Frequenti (FAQ)</h2>
            <p>Qui trovi risposte ad alcune delle domande più comuni sul kernel e il suo funzionamento.</p>

            <div class="faq-item">
                <div class="faq-question">
                    Il kernel è un sistema operativo completo?
                    <i class="fas fa-chevron-down"></i>
                </div>
                <div class="faq-answer">
                    <p>No, il **kernel** è il nucleo centrale del sistema operativo, ma non è il sistema operativo completo. Il sistema operativo include il kernel più un insieme di utility, librerie, interfacce utente (shell o GUI) e applicazioni che lavorano insieme per fornire un ambiente funzionale per l'utente.</p>
                </div>
            </div>

            <div class="faq-item">
                <div class="faq-question">
                    Posso programmare direttamente il kernel?
                    <i class="fas fa-chevron-down"></i>
                </div>
                <div class="faq-answer">
                    <p>In generale, no. La programmazione del kernel (sviluppo di driver, moduli kernel, ecc.) richiede conoscenze avanzate di programmazione a basso livello (spesso in C o Assembly) e una comprensione approfondita dell'architettura del sistema. È un compito complesso e potenzialmente rischioso per la stabilità del sistema.</p>
                </div>
            </div>

            <div class="faq-item">
                <div class="faq-question">
                    Qual è la differenza tra un kernel e un sistema operativo?
                    <i class="fas fa-chevron-down"></i>
                </div>
                <div class="faq-answer">
                    <p>Il **kernel** è il componente centrale e più importante del sistema operativo, che gestisce le risorse hardware e fornisce servizi di base. Il **sistema operativo (OS)** è l'insieme completo di software che gestisce l'hardware del computer e il software, fornendo servizi comuni per i programmi. Pensa al kernel come al motore di un'auto; l'OS è l'intera auto, completa di carrozzeria, ruote e interni.</p>
                </div>
            </div>

            <div class="faq-item">
                <div class="faq-question">
                    Perché il mio computer ha avuto un "kernel panic" o una "schermata blu"?
                    <i class="fas fa-chevron-down"></i>
                </div>
                <div class="faq-answer">
                    <p>Un "kernel panic" (su sistemi Unix/Linux/macOS) o una "schermata blu della morte" (BSOD su Windows) indica che il kernel ha rilevato un errore grave da cui non può recuperare in modo sicuro. Le cause comuni includono driver di dispositivo malfunzionanti, hardware difettoso (RAM o disco), software dannoso, o bug critici nel kernel stesso. Di solito, è un meccanismo di protezione per prevenire ulteriori danni.</p>
                </div>
            </div>

            <div class="faq-item">
                <div class="faq-question">
                    Come posso aggiornare il kernel del mio sistema operativo?
                    <i class="fas fa-chevron-down"></i>
                </div>
                <div class="faq-answer">
                    <p>Nella maggior parte dei casi, non è necessario aggiornare il kernel manualmente. Gli aggiornamenti del kernel vengono forniti come parte degli aggiornamenti regolari del sistema operativo. Su Windows, si aggiorna tramite Windows Update. Su Linux, si usano i gestori di pacchetto della tua distribuzione (es. <code>sudo apt update && sudo apt upgrade</code> per Ubuntu/Debian). È sempre consigliabile mantenere il sistema aggiornato per sicurezza e prestazioni.</p>
                </div>
            </div>
        </div>
    </section>

    <section id="curiosita" class="content-page">
      <div class="inner-content" data-aos="fade-up">
        <h2>Curiosità sul Kernel</h2>
        <p>Il kernel, pur essendo un componente "invisibile" per la maggior parte degli utenti, è un campo pieno di storie interessanti, personaggi influenti e sfide tecnologiche.</p>

        <h3>1. La Nascita di Linux: Un "Hobby" che ha Cambiato il Mondo</h3>
        <p>Nel 1991, **Linus Torvalds**, allora studente all'Università di Helsinki, iniziò a scrivere un kernel come "hobby". Voleva un sistema operativo simile a Unix che girasse sul suo nuovo PC con processore Intel 80386. Pubblicò il suo codice e lo mise a disposizione della comunità, chiedendo suggerimenti e collaborazioni.</p>
        <p>Quel piccolo hobby è diventato il kernel Linux, che oggi alimenta dal 70% all'80% degli smartphone mondiali (Android), l'80% dei server web, quasi il 100% dei supercomputer e un'infinità di dispositivi embedded. La sua crescita è una testimonianza del potere dello sviluppo open-source collaborativo.</p>
        <img src="https://picsum.photos/seed/linustorvalds/800/400" alt="Ritratto di Linus Torvalds" class="section-image">

        <h3>2. Il Dibattito Tanenbaum-Torvalds</h3>
        <p>Un famoso dibattito è scoppiato nel 1992 tra Linus Torvalds e **Andrew S. Tanenbaum** (creatore di Minix). Tanenbaum sosteneva che l'architettura monolitica di Linux fosse obsoleta e che i microkernel fossero il futuro. Torvalds difendeva il design di Linux, sostenendo che un monolitico potesse essere più efficiente e pratico per l'implementazione.</p>
        <p>Questo dibattito, spesso chiamato "The Great Debate", è ancora studiato nelle università e ha evidenziato le diverse filosofie di design dei sistemi operativi. Alla fine, entrambi i tipi di kernel hanno trovato il loro posto nel mondo tecnologico, dimostrando che non esiste un'unica "soluzione migliore" per tutte le esigenze.</p>
        <img src="https://picsum.photos/seed/debate/800/400" alt="Persone che discutono, metafora di dibattito" class="section-image">

        <h3>3. "Panic" del Kernel</h3>
        <p>Hai mai visto una "schermata blu della morte" su Windows o un messaggio di errore simile su Linux (spesso con "kernel panic")? Questo accade quando il kernel rileva un errore così grave da non poter continuare a funzionare in modo sicuro.</p>
        <p>Un kernel panic è essenzialmente il sistema operativo che si autodichiara incapace di recuperare da un errore interno. Spesso è causato da bug nel kernel stesso, driver di dispositivo difettosi o problemi hardware. È l'ultima risorsa per evitare danni maggiori ai dati o all'hardware.</p>
        <img src="https://picsum.photos/seed/bsod/800/400" alt="Schermata blu di errore" class="section-image">

        <h3>4. Kernel in Miniatura: I Dispositivi Embedded</h3>
        <p>Non solo desktop e server! Molti dispositivi intelligenti che usiamo quotidianamente contengono un kernel. Router Wi-Fi, smart TV, frigoriferi intelligenti, sistemi di controllo industriale e persino alcune lampadine intelligenti eseguono una versione ridotta di un kernel (spesso Linux o un RTOS - Real-Time Operating System) per gestire le loro funzioni.</p>
        <p>Questi kernel sono spesso ottimizzati per consumare poca energia, occupare poco spazio e garantire risposte in tempo reale, cruciali per il funzionamento di dispositivi come i robot o i sistemi di controllo veicolare.</p>
        <img src="https://picsum.photos/seed/iot/800/400" alt="Dispositivi IoT e embedded" class="section-image">

        <h3>5. Aggiornamenti del Kernel: Perché Sono Importanti</h3>
        <p>Ogni volta che il tuo sistema operativo installa un aggiornamento importante, c'è una buona probabilità che includa un aggiornamento del kernel. Questi aggiornamenti non sono solo per nuove funzionalità; sono vitali per la sicurezza e le prestazioni.</p>
        <p>Un kernel aggiornato può correggere vulnerabilità di sicurezza critiche, migliorare la compatibilità con nuovo hardware, ottimizzare le prestazioni e risolvere bug che potrebbero causare instabilità. Ecco perché è sempre consigliabile mantenere il kernel del proprio sistema aggiornato!</p>
        <img src="https://picsum.photos/seed/update/800/400" alt="Icona di aggiornamento software" class="section-image">
      </div>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 - Sito sul Kernel</p>
  </footer>

  <button onclick="topFunction()" id="backToTopBtn" title="Torna su"><i class="fas fa-arrow-up"></i></button>

  <script src="https://unpkg.com/aos@next/dist/aos.js"></script>
  <script>
    AOS.init({
      duration: 1000,
      once: true,
    });

    document.addEventListener('DOMContentLoaded', () => {
      const navLinks = document.querySelectorAll('nav.top-nav a');
      const contentPages = document.querySelectorAll('section.content-page');
      const backToTopBtn = document.getElementById("backToTopBtn");
      const loader = document.getElementById("loader");
      const searchInput = document.getElementById("search-input");
      const searchResultsList = document.getElementById("search-results-list");
      const themeSwitcher = document.getElementById("theme-switcher");
      let currentActivePageId = 'home';
      let currentSearchQuery = ''; // Memorizza l'ultima query di ricerca

      // Struttura per salvare il contenuto originale HTML di ogni sezione per la ricerca
      const allSectionsOriginalContent = {};
      contentPages.forEach(page => {
          allSectionsOriginalContent[page.id] = page.querySelector('.inner-content').innerHTML;
      });

      // --- Funzioni per il Loader ---
      function showLoader() {
        loader.style.display = 'block';
      }

      function hideLoader() {
        loader.style.display = 'none';
      }

      // --- Funzioni per il Sommario Dinamico (Table of Contents) ---
      const sectionsWithToc = ['cos-e', 'tipi', 'funzioni', 'esempi', 'glossario', 'curiosita'];

      function generateTableOfContents(pageElement) {
          if (!sectionsWithToc.includes(pageElement.id)) {
              return; // Non generare ToC per questa sezione
          }

          const innerContent = pageElement.querySelector('.inner-content');
          const h3Elements = innerContent.querySelectorAll('h3'); // Seleziona i sottotitoli H3

          if (h3Elements.length === 0) return; // Non ci sono sottotitoli per un sommario

          // Controlla se il sommario esiste già per questa pagina e, in tal caso, rimuovilo per rigenerarlo
          let tocContainer = innerContent.querySelector('.table-of-contents');
          if (tocContainer) {
              tocContainer.remove();
          }

          tocContainer = document.createElement('div');
          tocContainer.classList.add('table-of-contents');

          const tocTitle = document.createElement('h3');
          tocTitle.textContent = 'Indice della Sezione';
          tocContainer.appendChild(tocTitle);

          const tocList = document.createElement('ul');
          tocContainer.appendChild(tocList);

          h3Elements.forEach((h3, index) => {
              // Assicurati che ogni h3 abbia un ID per il link, se non ce l'ha già
              let id = h3.id;
              if (!id) {
                  // Genera un ID basato sul testo, normalizzato e con un indice per unicità
                  id = `${pageElement.id}-${normalizeText(h3.textContent).replace(/[^a-z0-9]+/g, '-')}-${index}`;
                  h3.id = id;
              }

              const listItem = document.createElement('li');
              const link = document.createElement('a');
              link.href = `#${id}`;
              link.textContent = h3.textContent;
              link.addEventListener('click', (e) => {
                  e.preventDefault();
                  // Scorri alla posizione dell'h3 con un offset per non coprirlo con l'header fisso
                  const offset = 100; // Spazio da lasciare dall'alto
                  const elementPosition = h3.getBoundingClientRect().top + pageElement.scrollTop;
                  pageElement.scrollTo({
                      top: elementPosition - offset,
                      behavior: 'smooth'
                  });
              });
              listItem.appendChild(link);
              tocList.appendChild(listItem);
          });

          // Inserisci il ToC dopo l'h2 principale della sezione
          const h2 = innerContent.querySelector('h2');
          if (h2) {
              innerContent.insertBefore(tocContainer, h2.nextSibling);
          } else {
              innerContent.prepend(tocContainer); // Se non c'è h2, mettilo all'inizio
          }
      }

      // --- Funzioni per la Navigazione delle Pagine ---
      function showPage(targetId, scrollToHighlight = false) {
        showLoader();

        const oldActivePage = document.querySelector('section.content-page.active');
        if (oldActivePage && oldActivePage.id !== targetId) {
          oldActivePage.classList.remove('active');
          oldActivePage.classList.add('leaving');

          // Assicurati che la pagina precedente sia completamente nascosta prima di mostrare la nuova
          oldActivePage.addEventListener('transitionend', function handler() {
            oldActivePage.classList.remove('leaving');
            oldActivePage.removeEventListener('transitionend', handler);
            oldActivePage.scrollTop = 0; // Reset scroll della pagina uscente
            backToTopBtn.classList.remove('show');
            backToTopBtn.style.display = 'none';
          }, { once: true });
        }

        const newActivePage = document.getElementById(targetId);
        if (newActivePage) {
          // Un piccolo ritardo per permettere alla transizione 'leaving' di iniziare
          setTimeout(() => {
            newActivePage.classList.add('active');
            currentActivePageId = targetId;
            hideLoader();

            // Aggiorna lo stato dei link di navigazione
            navLinks.forEach(link => {
              if (link.getAttribute('href') === '#' + targetId) {
                link.classList.add('active');
              } else {
                link.classList.remove('active');
              }
            });

            // Ripristina il contenuto originale della pagina corrente prima di rigenerare il ToC e gli highlights
            newActivePage.querySelector('.inner-content').innerHTML = allSectionsOriginalContent[newActivePage.id];

            // Genera il sommario se applicabile
            generateTableOfContents(newActivePage);

            // Applica evidenziazioni alla nuova pagina se c'è una query attiva
            if (currentSearchQuery) {
                highlightTextInPage(newActivePage, currentSearchQuery);
                if (scrollToHighlight) {
                    scrollToFirstHighlight(newActivePage);
                }
            }

          }, oldActivePage ? 600 : 0); // Ritardo basato sulla durata della transizione CSS (0.7s)
        } else {
          hideLoader();
        }
      }

      // Gestione dei click sui link di navigazione
      navLinks.forEach(anchor => {
        anchor.addEventListener('click', function (e) {
          e.preventDefault();
          const targetId = this.getAttribute('href').substring(1);
          showPage(targetId);
          history.pushState(null, null, '#' + targetId); // Aggiorna l'URL senza ricaricare
          searchInput.value = ''; // Pulisci la barra di ricerca quando si naviga
          searchResultsList.style.display = 'none'; // Nascondi i risultati
          currentSearchQuery = ''; // Resetta la query di ricerca attiva
        });
      });

      // Caricamento iniziale della pagina basato sull'hash nell'URL
      const initialHash = window.location.hash.substring(1);
      if (initialHash && document.getElementById(initialHash)) {
        showPage(initialHash);
      } else {
        showPage('home'); // Pagina predefinita se nessun hash o hash non valido
      }

      // --- Scrollspy: Evidenzia il link di navigazione della sezione corrente ---
      let throttleTimeout;
      function checkActiveNavLink() {
          if (throttleTimeout) return;

          throttleTimeout = setTimeout(() => {
              const currentScrollPos = window.scrollY || document.documentElement.scrollTop;
              let bestMatchId = currentActivePageId; // Mantieni l'attuale pagina attiva se non c'è un match migliore

              contentPages.forEach(page => {
                  if (page.classList.contains('active')) { // Solo per la pagina attiva
                      const innerContent = page.querySelector('.inner-content');
                      if (!innerContent) return;

                      // Calcola la posizione della sezione rispetto all'inizio del contenuto scrollabile
                      const pageScrollTop = page.scrollTop; // Scroll della sezione interna
                      const offset = 120; // Offset per considerare l'altezza dell'header e la padding

                      // Trova gli h2 e h3 all'interno della pagina attiva
                      const headings = innerContent.querySelectorAll('h2, h3');
                      let foundHeading = false;

                      headings.forEach(heading => {
                          const headingPos = heading.offsetTop; // Posizione dell'elemento rispetto al suo contenitore scrollabile
                          // Se il heading è visibile o appena passato (con un offset)
                          if (pageScrollTop + offset >= headingPos) {
                              // Se è un H2, cerchiamo il link di navigazione principale
                              if (heading.tagName === 'H2') {
                                  // Trova l'ID della sezione padre dell'H2
                                  let sectionId = heading.closest('section.content-page').id;
                                  let navLink = document.querySelector(`nav.top-nav a[href="#${sectionId}"]`);
                                  if (navLink) {
                                      navLinks.forEach(link => link.classList.remove('active'));
                                      navLink.classList.add('active');
                                      foundHeading = true;
                                  }
                              }
                              // Se è un H3 e c'è un ToC, potremmo volere evidenziare il link nel ToC
                              // Questa logica è più complessa e richiederebbe una gestione del ToC all'interno del nav
                              // Per ora, ci concentriamo sull'evidenziazione del link principale nella top-nav
                          }
                      });
                      // Se nessun heading specifico è stato trovato (es. all'inizio della pagina), assicurati che il link della pagina attiva sia evidenziato
                      if (!foundHeading) {
                           let navLink = document.querySelector(`nav.top-nav a[href="#${page.id}"]`);
                           if (navLink) {
                                navLinks.forEach(link => link.classList.remove('active'));
                                navLink.classList.add('active');
                           }
                      }
                  }
              });
              throttleTimeout = null;
          }, 150); // Aggiorna ogni 150ms per fluidità ma senza sovraccaricare
      }

      contentPages.forEach(page => {
        page.addEventListener('scroll', checkActiveNavLink);
      });


      // --- Pulsante "Torna su" ---
      contentPages.forEach(page => {
        page.addEventListener('scroll', () => {
          if (page.classList.contains('active')) { // Solo per la pagina attiva
            if (page.scrollTop > 150) { // Mostra il pulsante dopo 150px di scroll
              backToTopBtn.style.display = "block";
              setTimeout(() => backToTopBtn.classList.add('show'), 10); // Aggiungi classe per animazione
            } else {
              backToTopBtn.classList.remove('show');
              // Nascondi il pulsante solo dopo la fine dell'animazione di uscita
              backToTopBtn.addEventListener('transitionend', function handler() {
                if (!backToTopBtn.classList.contains('show')) {
                  backToTopBtn.style.display = "none";
                }
                backToTopBtn.removeEventListener('transitionend', handler);
              }, { once: true });
            }
          }
        });
      });

      function topFunction() {
        const activePage = document.querySelector('section.content-page.active');
        if (activePage) {
          activePage.scrollTo({
            top: 0,
            behavior: 'smooth' // Scorrimento fluido
          });
        }
      }
      window.topFunction = topFunction; // Rendi la funzione disponibile globalmente per onclick

      // --- Logica per l'accordion delle FAQ ---
      const faqQuestions = document.querySelectorAll('.faq-question');
      faqQuestions.forEach(question => {
        question.addEventListener('click', () => {
          const answer = question.nextElementSibling;
          const icon = question.querySelector('i');

          // Chiudi l'accordion se già aperto
          if (answer.style.maxHeight) {
            answer.style.maxHeight = null;
            answer.style.paddingTop = '0px';
            answer.style.paddingBottom = '0px';
            question.classList.remove('active');
          } else {
            // Chiudi tutti gli altri accordion aperti
            faqQuestions.forEach(otherQuestion => {
              if (otherQuestion !== question && otherQuestion.classList.contains('active')) {
                otherQuestion.classList.remove('active');
                otherQuestion.nextElementSibling.style.maxHeight = null;
                otherQuestion.nextElementSibling.style.paddingTop = '0px';
                otherQuestion.nextElementSibling.style.paddingBottom = '0px';
              }
            });
            // Apri l'accordion cliccato
            answer.style.maxHeight = answer.scrollHeight + "px"; // Imposta altezza per animazione
            answer.style.paddingTop = '15px';
            answer.style.paddingBottom = '15px';
            question.classList.add('active');
          }
        });
      });

      // --- Funzione di Ricerca Interna ---
      function normalizeText(text) {
          // Rimuove accenti e converte in minuscolo
          return text.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "");
      }

      function highlightText(text, phrase) {
          if (!phrase) return text;
          // Usa una regex per evidenziare tutte le occorrenze, ignorando la cassa
          const regex = new RegExp(phrase.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'gi'); // Escapa caratteri speciali per regex
          return text.replace(regex, '<span class="highlight">$&</span>');
      }

      function highlightTextInPage(pageElement, query) {
          const innerContent = pageElement.querySelector('.inner-content');
          // Ripristina il contenuto originale prima di evidenziare
          innerContent.innerHTML = allSectionsOriginalContent[pageElement.id];

          if (!query) return;

          // Utilizza un TreeWalker per processare solo i nodi di testo pertinenti
          const treeWalker = document.createTreeWalker(
              innerContent,
              NodeFilter.SHOW_TEXT,
              {
                  acceptNode: function(node) {
                      // Ignora i nodi di testo all'interno di tag script, style, o elementi già evidenziati
                      // O elementi del sommario (table-of-contents) per non evidenziare i link dell'indice
                      if (node.parentNode.nodeName === 'SCRIPT' ||
                          node.parentNode.nodeName === 'STYLE' ||
                          node.parentNode.classList.contains('highlight') ||
                          node.closest('.table-of-contents')) {
                          return NodeFilter.FILTER_SKIP;
                      }
                      return NodeFilter.FILTER_ACCEPT;
                  }
              },
              false
          );

          let node;
          const nodesToProcess = [];
          while (node = treeWalker.nextNode()) {
              nodesToProcess.push(node);
          }

          const normalizedQuery = normalizeText(query);
          nodesToProcess.forEach(node => {
              const text = node.nodeValue;
              if (normalizeText(text).includes(normalizedQuery)) {
                  const highlightedHTML = highlightText(text, query);
                  const tempDiv = document.createElement('div');
                  tempDiv.innerHTML = highlightedHTML;
                  while (tempDiv.firstChild) {
                      node.parentNode.insertBefore(tempDiv.firstChild, node);
                  }
                  node.parentNode.removeChild(node);
              }
          });
      }

      function scrollToFirstHighlight(pageElement) {
          const firstHighlight = pageElement.querySelector('.highlight');
          if (firstHighlight) {
              const offset = 100; // Spazio da lasciare dall'alto per l'header fisso
              // Calcola la posizione relativa della pagina e poi aggiungi lo scroll attuale
              const elementPosition = firstHighlight.getBoundingClientRect().top + pageElement.scrollTop;
              pageElement.scrollTo({
                  top: elementPosition - offset,
                  behavior: 'smooth'
              });
          }
      }

      function searchAndDisplayResults(query) {
        const normalizedQuery = normalizeText(query.trim());
        currentSearchQuery = query.trim(); // Aggiorna la query attiva per future navigazioni

        searchResultsList.innerHTML = ''; // Pulisci i risultati precedenti

        if (!normalizedQuery) {
            searchResultsList.style.display = 'none';
            // Quando la query è vuota, ripristina la pagina attiva al suo stato originale
            const activePage = document.getElementById(currentActivePageId);
            if (activePage) {
                activePage.querySelector('.inner-content').innerHTML = allSectionsOriginalContent[activePage.id];
                generateTableOfContents(activePage); // Rigenera il ToC
            }
            return;
        }

        let resultsFound = false;
        contentPages.forEach(page => {
            const pageId = page.id;
            const pageTitle = page.querySelector('h2') ? page.querySelector('h2').innerText : pageId;
            const pageContent = allSectionsOriginalContent[pageId]; // Usa il contenuto originale per la ricerca
            
            // Crea un div temporaneo per analizzare il testo senza modificare l'HTML originale della pagina
            const tempDiv = document.createElement('div');
            tempDiv.innerHTML = pageContent;
            const textContent = tempDiv.innerText;

            if (normalizeText(textContent).includes(normalizedQuery)) {
                resultsFound = true;
                const listItem = document.createElement('li');
                const link = document.createElement('a');
                link.href = `#${pageId}`;
                link.textContent = `Sezione: ${pageTitle}`;
                link.addEventListener('click', (e) => {
                    e.preventDefault();
                    showPage(pageId, true); // Naviga alla sezione e scorrere all'highlight
                    searchInput.value = query; // Mantiene la query nella barra di ricerca
                    searchResultsList.style.display = 'none'; // Nascondi i risultati
                });
                listItem.appendChild(link);
                searchResultsList.appendChild(listItem);
            }
        });

        if (resultsFound) {
            searchResultsList.style.display = 'block';
        } else {
            searchResultsList.style.display = 'none';
        }

        // Applica/rimuovi evidenziazioni e rigenera ToC solo sulla pagina attualmente attiva
        const activePage = document.getElementById(currentActivePageId);
        if (activePage) {
            // Ripristina il contenuto della pagina attiva e poi applica le modifiche
            activePage.querySelector('.inner-content').innerHTML = allSectionsOriginalContent[activePage.id];
            generateTableOfContents(activePage); // Rigenera il ToC per la pagina attiva
            if (currentSearchQuery) { // Se c'è una query, evidenzia
                highlightTextInPage(activePage, currentSearchQuery);
            }
        }
      }

      // Cattura l'input della ricerca al rilascio del tasto
      searchInput.addEventListener('keyup', (e) => {
          searchAndDisplayResults(e.target.value);
      });

      // Nascondi i risultati della ricerca quando si clicca fuori dall'input o dalla lista risultati
      document.addEventListener('click', (e) => {
          if (!searchInput.contains(e.target) && !searchResultsList.contains(e.target)) {
              searchResultsList.style.display = 'none';
          }
      });

      // --- Theme Switcher ---
      const savedTheme = localStorage.getItem('theme');
      if (savedTheme) {
          document.body.classList.add(savedTheme);
          if (savedTheme === 'light-theme') {
              themeSwitcher.querySelector('i').classList.remove('fa-sun');
              themeSwitcher.querySelector('i').classList.add('fa-moon');
          }
      } else {
          // Default: dark-theme e icona del sole
          document.body.classList.add('dark-theme'); // Assicurati che dark-theme sia sempre presente
          themeSwitcher.querySelector('i').classList.add('fa-sun');
      }

      themeSwitcher.addEventListener('click', () => {
          document.body.classList.toggle('light-theme');
          let theme = 'dark-theme';
          if (document.body.classList.contains('light-theme')) {
              theme = 'light-theme';
              themeSwitcher.querySelector('i').classList.remove('fa-sun');
              themeSwitcher.querySelector('i').classList.add('fa-moon');
          } else {
              themeSwitcher.querySelector('i').classList.remove('fa-moon');
              themeSwitcher.querySelector('i').classList.add('fa-sun');
          }
          localStorage.setItem('theme', theme);
      });

      // --- Animazione delle particelle sullo sfondo ---
      const canvas = document.getElementById('particles-js');
      const ctx = canvas.getContext('2d');
      let particles = [];
      const numParticles = 80;
      let maxDistance = 100;

      function resizeCanvas() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
      }
      window.addEventListener('resize', resizeCanvas);
      resizeCanvas();

      function Particle(x, y) {
        this.x = x;
        this.y = y;
        this.size = Math.random() * 2 + 1;
        this.speedX = Math.random() * 0.5 - 0.25;
        this.speedY = Math.random() * 0.5 - 0.25;
      }

      Particle.prototype.draw = function() {
        const highlightColor = getComputedStyle(document.documentElement).getPropertyValue('--highlight-color');
        ctx.fillStyle = highlightColor.replace('rgb', 'rgba').replace(')', ', 0.5)'); // Usa il colore del tema
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fill();
      };

      Particle.prototype.update = function() {
        this.x += this.speedX;
        this.y += this.speedY;

        if (this.x < 0 || this.x > canvas.width) {
          this.speedX *= -1;
        }
        if (this.y < 0 || this.y > canvas.height) {
          this.speedY *= -1;
        }
      };

      function initParticles() {
        particles = [];
        for (let i = 0; i < numParticles; i++) {
          const x = Math.random() * canvas.width;
          const y = Math.random() * canvas.height;
          particles.push(new Particle(x, y));
        }
      }

      function connectParticles() {
        const highlightColor = getComputedStyle(document.documentElement).getPropertyValue('--highlight-color');
        ctx.strokeStyle = highlightColor.replace('rgb', 'rgba').replace(')', ', 0.2)'); // Usa il colore del tema
        ctx.lineWidth = 1;

        for (let a = 0; a < particles.length; a++) {
          for (let b = a; b < particles.length; b++) {
            const distance = Math.sqrt(
              (particles[a].x - particles[b].x) ** 2 +
              (particles[a].y - particles[b].y) ** 2
            );
            if (distance < maxDistance) {
              ctx.beginPath();
              ctx.moveTo(particles[a].x, particles[a].y);
              ctx.lineTo(particles[b].x, particles[b].y);
              ctx.stroke();
            }
          }
        }
      }

      function animateParticles() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for (let i = 0; i < particles.length; i++) {
          particles[i].update();
          particles[i].draw();
        }
        connectParticles();
        requestAnimationFrame(animateParticles); // Chiamata ricorsiva per l'animazione fluida
      }

      initParticles();
      animateParticles();
    });
  </script>
</body>
</html>