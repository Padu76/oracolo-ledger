# Oracolo — ledger pubblico

Questo repository contiene il registro pubblico delle previsioni di [Oracolo](https://oracolo-sepia.vercel.app),
un esperimento pubblico di previsione tra cinque modelli di intelligenza artificiale su eventi reali italiani.
Nessun denaro, nessuna scommessa.

This repository is the public ledger of Oracolo's forecasts. Each batch of forecasts is committed here as a JSON file.
**The commit timestamp is GitHub's, not ours**: it proves the forecasts existed at that moment and were not rewritten later.

## Struttura / Layout

```
weeks/<settimana ISO>/batch-NNN.json
```

Ogni file contiene: le domande con criterio di risoluzione, il pacchetto di notizie fornito ai modelli,
le previsioni (risposta, fiducia, motivazione, data, hash), il modello richiesto e quello effettivamente servito da OpenRouter
(quando registrato) e lo sha256 del lotto precedente (`prev_file_sha256`), così i lotti formano una catena.

## Come verificare / How to verify

1. Guarda la data del commit su GitHub. / Check the commit date on GitHub.
2. `sha256sum weeks/<settimana>/batch-NNN.json` deve coincidere con quello mostrato in https://oracolo-sepia.vercel.app/it/ledger .
3. Per ogni previsione ricalcola l'hash:

   ```
   sha256_hex( question_id | model_id | answer | confidence | rationale_it | created_at_for_hash )
   ```

   con `|` come separatore. `created_at_for_hash` è già nel formato `YYYY-MM-DDTHH:MM:SS.ffffffZ` (UTC).
   Deve coincidere con il campo `hash` e con quello mostrato nella pagina pubblica della domanda.
4. Confronta le previsioni del file con quelle sul sito: devono essere identiche.

## Cosa prova e cosa non prova

Prova che le previsioni esistevano a quella data e non sono state riscritte dopo.
Non prova che il modello abbia risposto proprio in quel modo: per questo si registrano anche modello e provider effettivamente serviti.
I file di questo repository non si modificano: ogni lotto è un nuovo file.
