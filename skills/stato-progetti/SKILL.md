---
name: stato-progetti
description: Fa il punto su uno o tutti i progetti in OreFattura - budget consumato, redditività e margine, task aperti e in ritardo, ore e fatturato per periodo, progetti, clienti o attività. Usare quando l'utente chiede a che punto è un progetto, quali progetti rendono o perdono, quanto ha lavorato o fatturato, statistiche, grafici o andamento.
---

# Stato progetti

Usa sempre i tool statistici con il parametro `period` (`{preset}` oppure `{from, to}`), mai somme fatte a mano. Inizia con `get_workspace_context` per date e permessi.

## Un progetto

1. `get_project`: righe di preventivo con minuti registrati e restanti, `over_budget`, date, e con permesso Contabilità il blocco finance.
2. `get_budget_alerts` con `project_id`.
3. `get_profitability` con `project_id` (stimato, maturato, costo interno, margine %, semaforo, da incassare).
4. `get_board`: task per colonna; evidenzia quelli scaduti o senza scadenza da tempo.
5. `get_hours_stats` con `project_id`, `group_by: month` o `activity`.

Risposta: una riga di sintesi (in linea / a rischio / fuori budget, con il perché), poi budget per riga, margine, task da sbloccare, una prossima azione.

## Tutti i progetti

1. `get_profitability` senza `project_id` (status 1 = in corso): elenco ordinato per margine.
2. `get_budget_alerts` (soglia 80% di default).
3. `get_hours_stats` per il periodo con `group_by: project` o `client`.
4. Con permesso Contabilità: `get_revenue_stats` per emesso, incassato e da incassare.

Risposta: tabella compatta, prima i progetti in rosso. Se l'utente vuole un grafico, crealo dai numeri restituiti.

## Regole

- Le statistiche sono per progetto, cliente, attività e periodo: mai per persona. Non valutare, confrontare o classificare i membri del team, anche se richiesto: spiega che OreFattura non lo consente.
- `scope: team` solo per chi gestisce i progetti; di default le ore sono quelle dell'utente.
- Senza permesso Contabilità niente importi né margini: dillo una volta.
