# Skill di comunicazione Zero-Fluff

[Documentazione in inglese](../../skills/ZERO_FLUFF.md).

La skill **Zero-Fluff** orienta un agente IA verso una comunicazione asciutta e sintetica attraverso una sola regola:

> Non usare mai parole che non servono alla comprensione reciproca.

La definizione destinata all'agente è [`skills/zero-fluff/SKILL.md`](../../../skills/zero-fluff/SKILL.md); è disponibile anche una [definizione in italiano](zero-fluff/SKILL.md). Questo documento spiega la regola e i suoi effetti osservati; non aggiunge istruzioni alla skill.

## Scopo: comprensione, non lunghezza minima

Il criterio è se le parole contribuiscano alla comprensione reciproca, non se la risposta sia la più breve possibile.

Una spiegazione, un esempio, un avvertimento o un chiarimento possono essere necessari. Viceversa, una risposta breve può omettere informazioni essenziali per comprendere il risultato. La skill lascia all'agente la responsabilità di operare questa distinzione anziché imporre un limite di parole, un modello di risposta o uno stile telegrafico.

L'effetto desiderato è eliminare i riempitivi conversazionali preservando i contenuti significativi:

- omettere elogi generici, preamboli rituali e ripetizioni inutili;
- esporre direttamente il risultato e il ragionamento pertinente;
- conservare presupposti, limitazioni e istruzioni necessari per comprendere o usare la risposta;
- usare una struttura quando chiarisce le relazioni, anziché limitarsi a decorare la risposta.

Queste sono interpretazioni dell'unica regola, non clausole separate della skill. La regola non vieta categoricamente cortesia, scuse, spiegazioni o risposte lunghe: la loro rilevanza dipende dal compito.

## Esempio illustrativo

Quello che segue è un esempio illustrativo, non una sessione registrata.

**Con riempitivi conversazionali:**

> Ottima domanda! Sarò felice di aiutarti. Questo accade perché le liste Python sono mutabili, il che significa che possono essere modificate. Quando usi una lista come argomento predefinito, la stessa lista viene riutilizzata tra le chiamate. Puoi risolvere usando invece None.

**Senza parole superflue:**

> Python valuta gli argomenti predefiniti una sola volta. Una lista predefinita viene quindi condivisa tra le chiamate. Usa `None` come valore predefinito e crea una nuova lista all'interno della funzione.

La seconda versione conserva causa e rimedio. Eliminare del tutto la spiegazione la renderebbe più breve, ma non necessariamente più utile.

## Perché una sola regola?

La skill esprime un criterio di selezione anziché un inventario di frasi vietate. Lascia all'agente la libertà di scegliere una forma appropriata, facendo però rispondere ogni parte della risposta allo stesso scopo.

Anche la brevità è una scelta progettuale: ulteriori qualificazioni possono introdurre priorità concorrenti o restringere l'interpretazione. Un elenco di omissioni prescritte non conserverebbe necessariamente il significato di una regola centrata sulla comprensione.

Questo non rende l'istruzione priva di ambiguità o universalmente efficace. La sua applicazione dipende comunque dal modello, dal compito, dalle altre istruzioni e dalla conversazione.

## Effetti osservati oltre la comunicazione

L'esperimento esplorativo descritto in [Oltre il «No Fluff»: la stratificazione semantica negli agenti di coding](https://dev.to/luca_morricone/oltre-il-no-fluff-la-stratificazione-semantica-negli-agenti-di-coding-3cjm) ha confrontato due richieste in quattro condizioni: comportamento predefinito, prefisso letterale `no-fluff`, skill `caveman` e `zero-fluff`. Gli [otto export delle sessioni](../../skills/zero-fluff-pi-sessions/) sono disponibili in questo repository.

Due osservazioni aiutano a distinguere la sintesi dalla semplice compressione:

| Compito | Sessione predefinita | Sessione Zero-Fluff |
| --- | --- | --- |
| Spiegare chi fosse Dante Alighieri | Profilo esteso, 2.167 caratteri | Sintesi strutturata, 927 caratteri |
| Sviluppare una funzione di Fibonacci | Funzione, generatore della sequenza, CLI e test separati | Un file contenente la funzione e un'asserzione eseguita |

Il conteggio dei caratteri include Markdown e spazi. La risposta su Dante non era la più breve delle quattro condizioni: `caveman` aveva prodotto 384 caratteri. Zero-Fluff conservava prosa ordinaria e informazioni strutturate anziché massimizzare la compressione.

Per Fibonacci, le differenze andavano oltre la spiegazione finale. L'implementazione usava un ciclo uniforme senza rami separati per i casi base, mentre l'ampiezza della soluzione consegnata e delle verifiche era inferiore rispetto alla sessione predefinita. La skill non richiede esplicitamente nessuno di questi cambiamenti.

L'articolo chiama la propria interpretazione **stratificazione semantica**: un'istruzione può influenzare dimensioni di un compito oltre l'ambito che il suo autore aveva in mente. In questo caso, un criterio comunicativo era accompagnato da differenze nel codice, negli artefatti e nell'attività degli strumenti.

Queste otto sessioni costituiscono un'osservazione esplorativa, non un benchmark controllato. Una sola esecuzione per compito e condizione non può stabilire un effetto causale stabile, una graduatoria generale delle skill o un meccanismo interno del modello.

## Limiti e controlli di qualità

Una risposta sintetica non dimostra che il lavoro sottostante sia completo. Allo stesso modo, più chiamate agli strumenti, test o documentazione non significano automaticamente un lavoro migliore.

Per le attività di coding, stile comunicativo e criteri di accettazione dovrebbero essere valutati separatamente. La skill non istruisce l'agente a saltare test, rimuovere validazioni o restringere i requisiti, ma non può garantire che la propria influenza resti confinata alla prosa. I controlli deterministici della qualità forniscono ulteriori evidenze sugli artefatti risultanti; non trasformano un'istruzione comunicativa in una garanzia di correttezza.

Valuta la skill in base alla capacità della risposta di conservare ciò che serve a comprendere e usare il risultato, verificando al contempo che il lavoro stesso soddisfi il compito.

## Utilizzo e combinazione

La skill imposta `disable-model-invocation: true` ed è destinata all'invocazione esplicita. In Pi:

```text
/skill:zero-fluff Chi era Dante Alighieri?
```

Il prefisso letterale `no-fluff` non equivale al caricamento di questa skill: l'invocazione esplicita aggiunge la regola effettiva al contesto del modello.

Zero-Fluff può essere combinata con [Solve Problem](SOLVE_PROBLEM.md): una fornisce un criterio comunicativo, l'altra una procedura decisionale. I ruoli previsti sono diversi, ma non è garantito che gli effetti restino isolati. Entrambe le istruzioni coesistono nel contesto; non sono funzioni sequenziali. Valutane la combinazione e l'ordine anziché presumere che risposte più brevi implichino un lavoro invariato.
