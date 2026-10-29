---
name: registra-ore
description: Registra le ore lavorate in OreFattura come proposte da confermare, partendo da una frase libera ("ieri 3 ore sul sito Rossi"), da un elenco o dagli eventi del calendario. Usare quando l'utente chiede di segnare, registrare, inserire o compilare ore, il timesheet, la settimana o la giornata, o di riportare le riunioni del calendario sul timesheet.
---

# Registra ore

Trasforma ciò che l'utente ha fatto in righe di timesheet proposte in OreFattura. Le ore restano tratteggiate («AI», «Da confermare») finché l'utente non le conferma nell'app.

## 1. Contesto

1. Chiama `get_workspace_context`. Usa la data di oggi e i confini di settimana che restituisce: non dedurre la data da solo.
2. Se `can_log_time` è falso, fermati: spiega che senza costo orario nel profilo OreFattura non registra ore e che va impostato nel profilo.
3. Chiama `get_week_summary` per ogni settimana coinvolta, per sapere quanto è già pieno ogni giorno.

## 2. Trova la destinazione di ogni blocco di lavoro

Le ore si registrano solo su righe di preventivo a ore (hourly) o a giornata (daily) di progetti in corso.

- **Da frase libera**: cerca il progetto con `search` o `get_projects`, poi apri `get_project` e scegli il `project_activity_id` della riga giusta (aperta: senza `end_date` o con `end_date` futura). Se più righe sono plausibili, chiedi all'utente quale.
- **Da calendario**: usa il connettore calendario dell'utente per leggere gli eventi del periodo. Se nessun connettore calendario è disponibile, dillo e suggerisci di collegare Google Calendar (o Outlook) dalle impostazioni dei connettori di Claude; nel frattempo chiedi l'elenco delle attività a parole. Per ogni evento chiama `suggest_time_destination` con titolo e descrizione:
  - `confidence: high` → accetta;
  - `medium` o `low` → chiedi all'utente scegliendo tra i candidati restituiti;
  - `unmatched: true` → raccogli questi eventi e chiedi in **una sola domanda** su quale progetto e attività metterli. Mai a caso.
  - Salta eventi tutto il giorno, ferie, eventi rifiutati e blocchi personali, a meno che l'utente non dica il contrario.

## 3. Componi le righe

Per ogni riga: `project_activity_id`, `work_date`, `minutes`, eventuale `project_task_id`, `notes`.

- Minuti dalla durata dichiarata o dell'evento. Arrotonda solo se l'utente lo chiede.
- Note brevi e concrete. Per le riunioni: `Riunione: <titolo>`. Mai nomi, email o link dei partecipanti.
- Per gli eventi di calendario imposta `external_ref` (`gcal:<id istanza>`, `o365:<id>`, `ics:<uid>@<inizio>`): evita i doppioni.
- Confronta con `get_week_summary`: se un giorno supererebbe le ore per giornata del workspace, segnalalo e chiedi prima di proporre.

## 4. Riepiloga, poi proponi una volta

1. Mostra una tabella compatta: data, progetto, attività, durata, nota. Totale per giorno.
2. Su conferma, chiama `propose_time_entries` con tutte le righe (massimo 50 per chiamata).
3. Leggi l'esito di ogni riga: `already_present` e `already_proposed` non vanno riproposte; spiega le `invalid` con il loro messaggio.
4. Chiudi con il link al timesheet restituito e ricorda che le ore vanno confermate in OreFattura e che le ore non confermate non entrano in fattura.

## Correzioni

- Per spostare o correggere una riga esistente usa `propose_time_entry_update`; per toglierla `propose_time_entry_delete`.
- Per ripetere una settimana tipo usa `propose_week_copy`.
- Una proposta ancora in attesa si ritira con `withdraw_proposal` (elenco con `get_proposals`).
