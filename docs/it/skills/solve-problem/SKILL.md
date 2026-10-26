---
name: solve-problem-it
description: Risolvi problemi ricorsivamente mediante modellazione isomorfa al dominio e verifica iterativa.
disable-model-invocation: true
---

# Risolvi il problema

[Skill originale in inglese](../../../../skills/solve-problem/SKILL.md).

```text
Risolvi(problema, stato):
    Definisci un TestObiettivo falsificabile per l'esito osservabile del problema.
    Identifica i concetti di dominio del problema.
    Per ogni concetto:
        Specificane il contratto e gli invarianti senza ambiguità.

    Richiedi che ogni concetto rispecchi il dominio, faccia rispettare i propri invarianti, esponga solo il proprio contratto, nasconda il proprio meccanismo e abbia un nome univoco ed esatto.
    Richiedi che non vi siano concetti o indirezioni oltre il necessario.
    Verifica TestObiettivo(stato), i contratti osservabili, gli invarianti del dominio e le interazioni tra concetti mediante controlli falsificabili e avversariali.
    Se tutti i controlli vengono superati e tutti i requisiti sono soddisfatti:
        Restituisci stato.

    Se è necessaria una scomposizione:
        Scomponi il problema lungo i confini del dominio in parti ortogonali.
        Per ogni parte:
            stato = Risolvi(parte, stato).

    Genera azioni candidate per i requisiti non soddisfatti entro i vincoli del dominio, inclusa l'integrazione necessaria.
    Selezionale e applicale in base all'economia concettuale, non alla dimensione della modifica.
    Rimuovi concetti o indirezioni oltre il necessario.
    Restituisci Risolvi(problema, stato).
```
