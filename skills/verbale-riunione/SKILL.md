---
name: verbale-riunione
description: Trasforma appunti, trascrizione o evento di calendario di una riunione in OreFattura - ore sul progetto, verbale come nota di progetto e task in bacheca, tutto come proposte. Usare quando l'utente incolla appunti di una call, chiede di chiudere o verbalizzare una riunione, o di creare i task usciti da un incontro con un cliente.
---

# Verbale riunione

## 1. Contesto

1. `get_workspace_context`.
2. Individua il progetto: dal testo, dal titolo dell'evento (`suggest_time_destination` se c'è un evento di calendario) o chiedendo. Apri `get_project` per righe, colonne della bacheca e membri.

## 2. Prepara le tre proposte

**Ore.** Una riga per chi scrive (solo le ore dell'utente): durata della riunione, nota `Riunione: <titolo>`, `external_ref` se viene dal calendario. Se `can_log_time` è falso, salta questa parte e dillo.

**Nota di progetto** (`propose_project_note`). Testo semplice, massimo 2000 caratteri, datato:
- Decisioni prese
- Punti aperti
- Prossimi passi con scadenza
Niente nomi, email o recapiti dei partecipanti esterni. Le note di progetto hanno un tetto complessivo di 5000 caratteri: se `get_project_notes` mostra che è quasi pieno, accorcia e avvisa.

**Task** (`propose_task`). Uno per ogni azione concreta con un responsabile interno. Titolo con verbo all'infinito, descrizione breve, `due_date` se detta. `assignee_ids` solo tra i membri del progetto (`get_project_members`), solo se l'assegnazione è detta esplicitamente negli appunti: non scegliere tu a chi assegnare. Menzioni come `@{user:ID}`.

## 3. Riepiloga e proponi

Mostra le tre parti in un unico riepilogo; su conferma invia le proposte una volta sola. Chiudi con i link restituiti e ricorda che vanno confermate in OreFattura (le menzioni notificano solo alla conferma).
