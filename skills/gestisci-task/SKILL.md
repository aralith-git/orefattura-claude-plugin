---
name: gestisci-task
description: Crea, sposta, aggiorna e commenta i task nella bacheca dei progetti OreFattura, con checklist e menzioni, e mostra i task assegnati all'utente. Usare quando l'utente chiede di aggiungere un task o una to-do a un progetto, spostare una card, cambiare stato o scadenza, aggiungere una checklist o un commento, o cosa ha da fare oggi o questa settimana.
---

# Gestisci task

Inizia con `get_workspace_context`.

## I miei task

`get_my_tasks`: raggruppa per scadenza (scaduti, oggi, settimana, senza data) e progetto. Se l'utente vuole pianificare la giornata, confronta con le ore già registrate (`get_week_summary`) e proponi un ordine, senza registrare nulla.

## Creare

1. Trova il progetto (`search` / `get_projects`) e apri `get_board` per le colonne.
2. `propose_task`: `title` con verbo all'infinito, `description` breve, `board_column_id` (default la prima colonna), `status`, `due_date` se detta, `label_ids` se l'utente le nomina.
3. Assegnatari solo tra i «members» di `get_project_members` e solo se l'utente li indica: non scegliere a chi assegnare in base a carico o rendimento.
4. Più task insieme: riepilogo unico, poi una proposta per task.

## Aggiornare

- Spostare di colonna: `propose_task_move`.
- Stato, titolo, scadenza, assegnatari, etichette: `get_task`, poi `propose_task_update` con i soli campi che cambiano.
- Checklist: `propose_task_checklist`.
- Commento con menzioni: `propose_task_comment`, menzioni come `@{user:ID}` con gli id «mentionable»; notificano solo alla conferma.

Riepiloga prima, proponi una volta, chiudi con il link alla bacheca e il promemoria della conferma in OreFattura.
