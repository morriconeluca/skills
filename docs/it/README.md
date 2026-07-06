# Raccolta di Skill per Agenti IA

Le skill aiutano a modificare il comportamento, il ragionamento e i pattern comunicativi degli agenti IA per renderli collaboratori più efficaci, diretti e professionali.

Al momento, il repository propone una sola skill fondamentale incentrata sulla qualità della comunicazione.

_Nota: La localizzazione in italiano delle skill, del README e della documentazione ha il solo scopo di renderne più accessibile la comprensione. Le skill tradotte sono destinate alla sola lettura e consultazione da parte dell'utente, non all'esecuzione da parte dell'agente._

---

## Skill Disponibili

### 1. Comunicazione Non Sicofantica (`non-sycophantic`)

Gli assistenti IA tendono naturalmente ad assecondare le scelte dell'utente, anche quando queste sono inefficienti, non ottimali o tecnicamente errate (un fenomeno noto come "sicofania" dell'IA).

La skill **Non Sicofantica** impone all'agente di:

- Mantenere un tono asciutto e tra pari (collaboratore alla pari).
- Valutare criticamente le richieste dell'utente prima di rispondere.
- Rifiutare in modo educato ma fermo le idee mediocri e proporre alternative migliori.
- Evitare preamboli inutili, convenevoli e riempitivi conversazionali.

Per vedere come funziona questa skill in pratica, consulta la documentazione specifica:
👉 **[Documentazione della Skill Non Sicofantica](skills/NON_SYCOPHANTIC.md)**

---

## Struttura del Progetto

```text
.
├── README.md                    # README principale (Inglese)
├── docs/
│   ├── skills/
│   │   └── NON_SYCOPHANTIC.md   # Documentazione dettagliata e confronti (Inglese)
│   └── it/                      # Localizzazione italiana (solo consultazione)
│       ├── README.md            # Questo file (Italiano)
│       ├── skills/
│       │   └── NON_SYCOPHANTIC.md # Documentazione in Italiano
│       └── non-sycophantic/
│           └── SKILL.md         # Definizione della skill in Italiano
└── skills/
    └── non-sycophantic/
        └── SKILL.md             # Definizione della skill (Inglese)
```

---

## Principi di Progettazione delle Skill

Quando scrivi o contribuisci con nuove skill a questa raccolta, segui questi principi fondamentali:

- **Brevità e Sintesi**: Mantieni le istruzioni brevi. I testi lunghi aggiungono rumore nel contesto e riducono l'attenzione del modello sulle regole chiave.
- **Linguaggio Imperativo e Privo di Ambiguità**: Scrivi istruzioni dirette e imperative. Comandi chiari (es. "Non fare mai X") producono risultati più costanti rispetto a suggerimenti passivi.
- **Rimozione della Prospettiva Umana**: Non rivolgerti al modello come se fosse un collega umano. Elimina convenevoli, sfumature emotive e spiegazioni. Concentrati solo su regole che vincolano direttamente l'output.
- **Precisione Semantica**: Usa parole precise invece di lunghe spiegazioni. Un singolo termine esatto può attivare comportamenti complessi. Scegli le parole con attenzione: ogni termine può introdurre interpretazioni indesiderate.
- **Logica Strutturata**: Usa strutture logiche come condizioni If/Else, elenchi o sequenze numerate. I modelli elaborano le relazioni causali meglio se strutturate in questi formati che in paragrafi discorsivi.
- **Integrazione dei Model Priors**: Non contrastare l'allineamento nativo del modello (RLHF). Non puoi sovrascrivere del tutto il suo addestramento. Invece di scrivere regole per aggirare questa predisposizione, inserisci dei vincoli. Per dettagli su questo approccio, consulta la [documentazione di non-sycophantic](skills/NON_SYCOPHANTIC.md#3-condizione-di-uscita-e-bias-del-modello) dove si evita che il modello assecondi subito l'utente costringendolo a opporsi almeno due volte (`Non uscire mai... alla prima scelta forzata`).
- **Progettazione per il Non-Determinismo**: Accetta risposte non deterministiche. Progettare una skill significa definire confini e limiti di comportamento, non pretendere un unico percorso rigido di risposta.

---

## Come Utilizzare le Skill

I sistemi di gestione degli agenti (come [Pi](https://pi.dev/)) possono caricare le skill su richiesta (on-demand).

Consiglio vivamente l'uso di **Pi** agli ingegneri del software che desiderano il massimo controllo sul proprio agente. Le skill personalizzate aggiunte al contesto funzionano al meglio con strumenti che hanno un system prompt succinto e tendente allo zero. Al contrario, all'interno di framework di agenti più pesanti (con system prompt complessi e voluminosi), le skill aggiunte dal singolo ingegnere rischiano facilmente di entrare in conflitto con le istruzioni di base e devono comunque competere con un contesto già colmo di rumore.

### Installazione / Configurazione

Copia la cartella della skill desiderata in una delle seguenti posizioni nel tuo progetto o ambiente globale:

- **A livello di progetto**: `.agents/skills/` o `.pi/skills/`
- **A livello globale**: `~/.agents/skills/` o `~/.pi/agent/skills/`

Una volta posizionata, il sistema di gestione dell'agente rileverà automaticamente la descrizione della skill e caricherà le istruzioni complete non appena viene rilevata una richiesta pertinente.

## Licenza

Questo progetto è open-source e distribuito sotto Licenza MIT.
