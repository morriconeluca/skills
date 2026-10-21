# Raccolta di Skill per Agenti IA

Le skill aiutano a modificare il comportamento, il ragionamento e i pattern comunicativi degli agenti IA per renderli collaboratori più efficaci, diretti e professionali.

Al momento, il repository propone skill fondamentali incentrate sulla qualità della comunicazione.

_Nota: La localizzazione in italiano delle skill, del README e della documentazione ha il solo scopo di renderne più accessibile la comprensione. Le skill tradotte sono destinate alla sola lettura e consultazione da parte dell'utente, non all'esecuzione da parte dell'agente._

---

## Skill Disponibili

### 1. Comunicazione Non Sicofantica (`non-sycophantic`)

Gli assistenti IA tendono naturalmente ad assecondare le scelte dell'utente, anche quando queste sono inefficienti, non ottimali o tecnicamente errate (un fenomeno noto come "sicofanzia" dell'IA).

La skill **Non Sicofantica** impone all'agente di:

- Mantenere una comunicazione alla pari (collaboratore alla pari) e non sicofantica.
- Valutare criticamente le richieste dell'utente prima di rispondere.
- Fornire una valutazione onesta e cruda, rifiutare le idee mediocri e proporre prospettive alternative.
- Porre domande pertinenti quando necessario.
- Resistere alle insistenze dell'utente senza cedere prematuramente (non uscire mai dal ciclo alla prima e seconda scelta forzata).
- Non dare mai priorità alla cortesia rispetto alla contraddizione logica.

Per vedere come funziona questa skill in pratica, consulta la documentazione specifica:
👉 **[Documentazione della Skill Non Sicofantica](skills/NON_SYCOPHANTIC.md)**

### 2. Comunicazione Zero-Fluff (`zero-fluff`)

Gli assistenti IA tendono naturalmente a inserire convenevoli, preamboli di cortesia, scuse e spiegazioni prolisse prive di reale valore semantico.

La skill **Zero-Fluff** impone una comunicazione orientata alla sintesi guidata da un'unica regola imperativa:

> _"Non usare mai parole che non servono alla comprensione reciproca."_

La skill costringe l'agente a:

- Mantenere uno stile comunicativo asciutto e orientato alla sintesi.
- Eliminare convenevoli, scuse, riempitivi conversazionali e preamboli cerimoniosi.
- Massimizzare la densità informativa concentrandosi unicamente sulla comprensione reciproca.

---

## Struttura del Progetto

```text
.
├── README.md                           # README principale (Inglese)
├── docs/
│   ├── skills/
│   │   ├── NON_SYCOPHANTIC.md          # Documentazione dettagliata e confronti (Inglese)
│   │   └── zero-fluff-pi-sessions/     # Sessioni di valutazione registrate (HTML)
│   └── it/                             # Localizzazione italiana (solo consultazione)
│       ├── README.md                   # Questo file (Italiano)
│       └── skills/
│           ├── NON_SYCOPHANTIC.md      # Documentazione in Italiano
│           ├── non-sycophantic/
│           │   └── SKILL.md            # Definizione della skill in Italiano
│           └── zero-fluff/
│               └── SKILL.md            # Definizione della skill in Italiano
└── skills/
    ├── non-sycophantic/
    │   └── SKILL.md                    # Definizione della skill (Inglese)
    └── zero-fluff/
        └── SKILL.md                    # Definizione della skill (Inglese)
```

---

## Principi di Progettazione delle Skill

Quando scrivi o contribuisci con nuove skill a questa raccolta, segui questi principi fondamentali:

- **Brevità e Sintesi**: Mantieni le istruzioni brevi. I testi lunghi aggiungono rumore nel contesto e riducono l'attenzione del modello sulle regole chiave.
- **Linguaggio Imperativo e Privo di Ambiguità**: Scrivi istruzioni dirette e imperative. Comandi chiari (es. "Non fare mai X") producono risultati più costanti rispetto a suggerimenti passivi.
- **Rimozione della Prospettiva Umana**: Non rivolgerti al modello come se fosse un collega umano. Elimina convenevoli, sfumature emotive e spiegazioni. Concentrati solo su regole che vincolano direttamente l'output.
- **Precisione Semantica**: Usa parole precise invece di lunghe spiegazioni. Un singolo termine esatto può attivare comportamenti complessi. Scegli le parole con attenzione: ogni termine può introdurre interpretazioni indesiderate.
- **Logica Strutturata**: Usa strutture logiche come condizioni If/Else, elenchi o sequenze numerate. I modelli elaborano le relazioni causali meglio se strutturate in questi formati che in paragrafi discorsivi.
- **Integrazione dei Model Priors**: Non contrastare l'allineamento nativo del modello (RLHF). Non puoi sovrascrivere del tutto il suo addestramento. Invece di scrivere regole per aggirare questa predisposizione, inserisci dei vincoli. Per dettagli su questo approccio, consulta la [documentazione di non-sycophantic](skills/NON_SYCOPHANTIC.md#3-condizione-di-uscita-e-bias-del-modello) dove si evita che il modello assecondi subito l'utente costringendolo a opporsi alle scelte forzate (`Non uscire mai... alla prima e seconda scelta forzata`).
- **Progettazione per il Non-Determinismo**: Accetta risposte non deterministiche. Progettare una skill significa definire confini e limiti di comportamento, non pretendere un unico percorso rigido di risposta.

---

## Come Utilizzare le Skill

Gli agent harness per la codifica (come [Pi](https://pi.dev/)) possono caricare le skill su richiesta (on-demand).

Consiglio vivamente l'uso di **Pi** agli ingegneri del software che desiderano il massimo controllo sul proprio agente. Le skill personalizzate aggiunte al contesto funzionano al meglio con strumenti che hanno un system prompt succinto e tendente allo zero. Al contrario, all'interno di framework di agenti più pesanti (con system prompt complessi e voluminosi), le skill aggiunte dal singolo ingegnere rischiano facilmente di entrare in conflitto con le istruzioni di base e devono comunque competere con un contesto già colmo di rumore.

### Installazione / Configurazione

Copia la cartella della skill desiderata in una delle seguenti posizioni nel tuo progetto o ambiente globale:

- **A livello di progetto**: `.agents/skills/` o `.pi/skills/`
- **A livello globale**: `~/.agents/skills/` o `~/.pi/agent/skills/`

Una volta posizionata, l'harness dell'agente rileverà automaticamente la descrizione della skill e caricherà le istruzioni complete non appena viene rilevata una richiesta pertinente.

## Licenza

Questo progetto è open-source e distribuito sotto Licenza MIT.
