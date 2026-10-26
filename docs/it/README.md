# Raccolta di skill per agenti IA

Le skill aiutano a modificare il comportamento, il ragionamento e le modalità comunicative degli agenti IA per renderli collaboratori più efficaci, diretti e professionali.

Questo repository propone skill dedicate alla qualità della comunicazione e alla risoluzione autonoma dei problemi.

[README in inglese](../../README.md).

---

## Skill disponibili

### 1. Comunicazione non sicofantica (`non-sycophantic`)

Gli assistenti IA tendono naturalmente ad assecondare le scelte dell'utente, anche quando queste sono non ottimali, inefficienti o tecnicamente errate: un fenomeno noto come sicofanzia dell'IA.

La skill **Non-Sycophantic** impone all'agente di:

- Mantenere un tono alla pari, da collaboratore.
- Valutare criticamente le richieste dell'utente prima di rispondere.
- Fornire una valutazione onesta e cruda, rifiutare idee mediocri e proporre prospettive alternative.
- Porre domande pertinenti quando necessario.
- Resistere alle insistenze dell'utente senza cedere prematuramente: non uscire mai dal ciclo alla prima e alla seconda scelta forzata.
- Non dare mai priorità alla cortesia rispetto alla contraddizione logica.

Per vedere come funziona questa skill in pratica, consulta la documentazione dettagliata:
👉 **[Documentazione della skill Non-Sycophantic](skills/NON_SYCOPHANTIC.md)**

[Definizione in italiano](skills/non-sycophantic/SKILL.md).

### 2. Comunicazione Zero-Fluff (`zero-fluff`)

Gli assistenti IA tendono naturalmente a inserire riempitivi conversazionali, preamboli di cortesia, scuse e spiegazioni prolisse prive di valore semantico.

La skill **Zero-Fluff** impone una comunicazione orientata alla sintesi, governata da un'unica regola imperativa:

> _«Non usare mai parole che non servono alla comprensione reciproca.»_

La skill guida l'agente a:

- Mantenere uno stile comunicativo asciutto, conciso e orientato alla sintesi.
- Rimuovere le formulazioni che non aggiungono valore alla comprensione reciproca.
- Conservare spiegazioni, presupposti e limitazioni quando sono necessari.

Il criterio è una sintesi utile, non la risposta più breve possibile. Le sessioni esplorative hanno mostrato differenze anche nel codice e nell'attività degli strumenti: non si dovrebbe quindi presumere che la sua influenza si fermi alla prosa.

**[Documentazione della skill Zero-Fluff](skills/ZERO_FLUFF.md)**

[Definizione in italiano](skills/zero-fluff/SKILL.md).

### 3. Solve Problem (`solve-problem`)

La skill **Solve Problem** fornisce una procedura decisionale ricorsiva pensata per migliorare il modo in cui un agente sceglie e applica azioni, senza prescrivere una particolare implementazione né privarlo della libertà di giudizio.

La procedura guida l'agente a:

- Definire un obiettivo falsificabile e specificare concetti di dominio, contratti e invarianti.
- Verificare lo stato corrente prima di decidere se modificarlo.
- Scomporre il problema quando necessario e integrare i risultati.
- Selezionare le azioni in base all'economia concettuale, non alla dimensione della modifica.
- Riesaminare lo stato risultante finché l'obiettivo e i requisiti non sono soddisfatti.

Per le attività di coding, gli strumenti deterministici di controllo qualità forniscono ulteriori evidenze e feedback. La skill guida il giudizio; non sostituisce i controlli vincolanti di qualità e non garantisce la correttezza.

**[Documentazione della skill Solve Problem](skills/SOLVE_PROBLEM.md)**

[Definizione in italiano](skills/solve-problem/SKILL.md).

---

## Struttura del progetto

```text
.
├── README.md                           # README principale (inglese)
├── docs/
│   ├── skills/
│   │   ├── NON_SYCOPHANTIC.md          # Documentazione e confronti (inglese)
│   │   ├── SOLVE_PROBLEM.md            # Processo decisionale, principi e limiti (inglese)
│   │   ├── ZERO_FLUFF.md               # Criterio comunicativo, osservazioni e limiti (inglese)
│   │   └── zero-fluff-pi-sessions/     # Sessioni di valutazione registrate (HTML)
│   └── it/                             # Localizzazione italiana
│       ├── README.md                   # Questo file
│       └── skills/
│           ├── NON_SYCOPHANTIC.md      # Documentazione tradotta
│           ├── SOLVE_PROBLEM.md        # Documentazione tradotta
│           ├── ZERO_FLUFF.md           # Documentazione tradotta
│           ├── non-sycophantic/
│           │   └── SKILL.md            # Definizione tradotta
│           ├── solve-problem/
│           │   └── SKILL.md            # Definizione tradotta
│           └── zero-fluff/
│               └── SKILL.md            # Definizione tradotta
└── skills/
    ├── non-sycophantic/
    │   └── SKILL.md                    # Definizione e istruzioni operative (inglese)
    ├── solve-problem/
    │   └── SKILL.md                    # Procedura ricorsiva di risoluzione (inglese)
    └── zero-fluff/
        └── SKILL.md                    # Definizione e istruzioni operative (inglese)
```

---

## Principi di progettazione delle skill

Quando scrivi o contribuisci con nuove skill a questa raccolta, segui questi principi fondamentali:

- **Brevità**: mantieni le istruzioni brevi. I testi lunghi aggiungono rumore e riducono l'attenzione del modello sulle direttive chiave.
- **Linguaggio imperativo e privo di ambiguità**: scrivi istruzioni dirette e imperative. Comandi chiari, come «Non fare mai X», producono risultati più costanti rispetto a suggerimenti passivi.
- **Decentramento della prospettiva umana**: non rivolgerti al modello come se fosse un collega umano. Elimina riempitivi conversazionali, connotazioni emotive e spiegazioni. Concentrati solo sulle regole che vincolano direttamente l'output.
- **Precisione semantica**: usa parole precise anziché lunghe spiegazioni. Un singolo termine esatto può attivare comportamenti complessi. Scegli però le parole con attenzione: ciascuna può introdurre effetti collaterali o interpretazioni indesiderate.
- **Logica strutturata**: usa strutture logiche come condizioni If/Else, elenchi o sequenze numerate. I modelli elaborano le relazioni causali meglio in formati strutturati che in paragrafi prolissi.
- **Riconoscimento delle predisposizioni native del modello**: non contrastare l'allineamento nativo del modello (RLHF). Non puoi sovrascrivere completamente il suo addestramento. Anziché scrivere regole per aggirare questo allineamento, introduci dei vincoli. Per i dettagli, consulta la [documentazione di non-sycophantic](skills/NON_SYCOPHANTIC.md#3-condizione-di-uscita-e-bias-del-modello), dove si impedisce al modello di cedere all'utente costringendolo a opporsi alle scelte forzate (`Non uscire mai... alla prima e seconda scelta forzata`).
- **Progettazione per il non determinismo**: aspettati risposte non deterministiche. Progettare una skill significa definire confini e vincoli comportamentali, non pretendere un unico percorso fisso di risposta.

---

## Come utilizzare le skill

Gli ambienti per agenti IA, come [Pi](https://pi.dev/), possono caricare le skill su richiesta.

Consiglio vivamente **Pi** agli ingegneri del software che desiderano il massimo controllo sui propri agenti IA. Le skill personalizzate aggiunte al contesto funzionano al meglio con strumenti dotati di un system prompt succinto, quasi nullo. Al contrario, i framework per agenti più pesanti, con system prompt ampi e molto strutturati, sono soggetti a conflitti tra istruzioni, e le skill personalizzate devono competere con il rumore già presente.

### Installazione e configurazione

Copia la directory della skill desiderata da [`skills/`](../../skills/) in una delle seguenti posizioni nel tuo progetto o ambiente globale.

- **A livello di progetto**: `.agents/skills/` o `.pi/skills/`
- **A livello globale**: `~/.agents/skills/` o `~/.pi/agent/skills/`

Una volta posizionata, l'ambiente dell'agente può rilevare la skill. Il comportamento di caricamento dipende dall'ambiente e dalla configurazione della skill: il rilevamento non implica necessariamente l'attivazione automatica.

`zero-fluff` e `solve-problem` sono configurate per l'invocazione esplicita (`disable-model-invocation: true`). In Pi:

```text
/skill:zero-fluff Chi era Dante Alighieri?
/skill:solve-problem Sviluppa un modulo per la prenotazione delle scorte in memoria.
```

Consulta la [documentazione di Solve Problem](skills/SOLVE_PROBLEM.md) per il processo decisionale e il suo rapporto con i controlli deterministici della qualità.

## Licenza

Questo progetto è open-source e distribuito sotto licenza MIT.
