# Skill Solve Problem

[Documentazione in inglese](../../skills/SOLVE_PROBLEM.md).

La skill **Solve Problem** fornisce una procedura decisionale ricorsiva pensata per migliorare il modo in cui un agente IA sceglie e applica azioni per risolvere un problema. Combina modellazione orientata al dominio, economia concettuale e verifica iterativa senza prescrivere un'implementazione né privare l'agente della libertà di giudizio.

La definizione destinata all'agente è [`skills/solve-problem/SKILL.md`](../../../skills/solve-problem/SKILL.md); è disponibile anche una [definizione in italiano](solve-problem/SKILL.md). Questo documento ne spiega la progettazione; non aggiunge istruzioni alla skill.

## Scopo: guidare il giudizio, non sostituirlo

Risolvere un problema richiede più della semplice esecuzione di controlli. Qualcuno deve individuare i concetti rilevanti, definirne le responsabilità, scegliere un approccio e decidere cosa verificare. Contratti e test sono spesso prodotti di questo lavoro, non specifiche complete disponibili in anticipo.

La skill interviene su questo livello decisionale. I suoi principi guidano l'agente nella definizione di una soluzione e dei relativi criteri di accettazione; il feedback della verifica orienta poi le decisioni successive.

Sviluppata originariamente per agenti di coding, la procedura usa termini generali — problema, stato, obiettivo, concetti, azioni — e non è limitata al software. La sua utilità al di fuori del coding resta da valutare.

## Principi di modellazione

| Principio | Funzione prevista |
| --- | --- |
| **Corrispondenza con il dominio** | Fare in modo che i concetti e le relazioni della soluzione riflettano il problema anziché dettagli implementativi accidentali. |
| **Contratti e invarianti** | Specificare cosa garantisce ogni concetto e quali proprietà devono essere rispettate. |
| **Confini di responsabilità** | Esporre il contratto nascondendo il meccanismo, affinché le interazioni non dipendano inutilmente dai dettagli interni. |
| **Precisione dei nomi** | Attribuire ai concetti nomi univoci ed esatti che ne esprimano significato e confini. |
| **Economia concettuale** | Evitare o rimuovere concetti e indirezioni non necessari senza confondere la semplicità con una modifica piccola. |

**«Economia concettuale, non dimensione della modifica» è una distinzione importante.** Una patch minuscola può preservare un modello confuso; una modifica più ampia può produrre una soluzione più semplice e coerente. La skill lascia all'agente la responsabilità di valutare cosa sia necessario entro i vincoli del problema.

## Come funziona la procedura ricorsiva

`Solve(problem, state)` opera sullo stato corrente, che può già contenere una soluzione completa, una soluzione incompleta o dei difetti.

1. **Definire l'obiettivo osservabile.** Stabilire un `GoalTest` falsificabile: un esito il cui mancato raggiungimento possa essere dimostrato da evidenze.
2. **Identificare e specificare i concetti.** Definirne contratti e invarianti senza ambiguità e stabilire i requisiti di modellazione che le azioni candidate devono rispettare.
3. **Verificare lo stato corrente.** Controllare l'obiettivo, i contratti osservabili, gli invarianti del dominio e le interazioni tra concetti. I controlli falsificabili possono fallire alla luce di evidenze; i controlli avversariali mettono attivamente alla prova la soluzione anziché limitarsi a mostrarne l'uso previsto.
4. **Restituire invariato uno stato adeguato.** Se i controlli vengono superati e tutti i requisiti sono soddisfatti, non occorre intervenire.
5. **Scomporre quando necessario.** Suddividere il problema lungo i confini del dominio e applicare la stessa procedura a ogni parte, propagando lo stato risultante. La scomposizione è condizionale, non un obbligo di inventare altri componenti.
6. **Scegliere e applicare azioni.** Generare candidate per i requisiti non soddisfatti, inclusa l'integrazione delle parti. Selezionare in base all'economia concettuale e rimuovere concetti o indirezioni non necessari.
7. **Rientrare nella procedura.** Applicare `Solve` allo stato aggiornato, affinché la verifica successiva determini se occorrono altre azioni.

La ricorsione serve a due scopi: risolvere problemi più piccoli e riesaminare il problema originale dopo le modifiche. Risolvere le sole parti non dimostra che la loro integrazione soddisfi il problema nel suo insieme.

## Autonomia e precisione semantica

La procedura specifica una struttura decisionale, non un percorso predeterminato. Non prescrive un linguaggio, un'architettura, un algoritmo, un framework di test, un numero di alternative o un grado di scomposizione.

Una scelta progettuale aperta non costituisce automaticamente un'informazione mancante che richiede una domanda. Specificare un contratto può essere parte della soluzione del problema, anziché un prerequisito che l'utente deve soddisfare.

La formulazione è volutamente compatta. L'approccio di sviluppo si basa sull'ipotesi discussa in [Oltre il «No Fluff»: la stratificazione semantica negli agenti di coding](https://dev.to/luca_morricone/oltre-il-no-fluff-la-stratificazione-semantica-negli-agenti-di-coding-3cjm): un'istruzione può influenzare più dimensioni del lavoro dell'agente di quante il suo autore intendesse. Aggiungere una qualificazione apparentemente utile può riorientare le scelte implementative, la verifica o il comportamento esplorativo.

Per questo, modalità di fallimento plausibili non sono, da sole, ragioni sufficienti per aggiungere prescrizioni. Le modifiche dovrebbero affrontare carenze osservate ed essere valutate attraverso esecuzioni ripetute, anziché presumere che un testo più esplicito migliori sempre il comportamento.

## Controlli deterministici della qualità

Per le attività di coding, la skill è pensata per affiancare gli strumenti deterministici di controllo qualità, non per sostituirli:

- compilatori e controllori dei tipi;
- linter e analisi statica;
- specifiche eseguibili e test di regressione;
- test basati su proprietà, di integrazione e di mutazione, quando appropriati.

Il ciclo di feedback è:

> **La skill guida le decisioni → gli strumenti producono evidenze → l'agente rivede le proprie decisioni.**

Un progetto può imporre criteri vincolanti di qualità sul risultato senza predeterminare le scelte implementative dell'agente. La skill, da sola, non installa strumenti, non impone controlli vincolanti e non garantisce che l'agente esegua ogni passaggio.

Occorre inoltre distinguere l'esecuzione deterministica dall'adeguatezza semantica: un test può verificare in modo affidabile la proprietà sbagliata e una suite superata può omettere un'interazione importante. Il feedback degli strumenti rafforza il processo, ma non garantisce una specifica completa o un risultato ottimale.

## Osservazioni e limiti

Le sessioni esplorative di sviluppo hanno messo alla prova tre comportamenti utili: costruire una soluzione da uno stato vuoto, preservare una soluzione già adeguata e correggere un difetto introdotto senza indebolire i test esistenti.

Altre sessioni hanno mostrato variazioni negli algoritmi e nella copertura dei casi limite anche con istruzioni identiche. Queste osservazioni non costituiscono un benchmark controllato e non dimostrano che la skill abbia causato miglioramenti rispetto a un agente che opera senza di essa.

Lo pseudocodice è un'istruzione in linguaggio naturale rivolta a un LLM, non un programma eseguito da un interprete. Non garantisce la terminazione, la completezza della verifica, la fedeltà della modellazione al dominio o il rispetto di ogni passaggio. Il suo valore dovrebbe essere valutato attraverso le scelte osservabili, gli artefatti prodotti, le modifiche non necessarie e la solidità dei controlli indipendenti, non semplicemente attraverso il numero di test o di chiamate agli strumenti.

## Utilizzo

Carica esplicitamente la skill per affrontare un problema. In Pi:

```text
/skill:solve-problem Sviluppa un modulo per la prenotazione delle scorte in memoria.
```

La skill imposta `disable-model-invocation: true`: è quindi destinata all'invocazione esplicita anziché all'attivazione automatica da parte del modello.

Può essere combinata con `zero-fluff`, ma le istruzioni coesistono nel contesto anziché essere eseguite come una pipeline deterministica. La loro interazione e il loro ordine vanno valutati, non considerati neutri a priori.
