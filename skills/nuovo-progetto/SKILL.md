---
name: nuovo-progetto
description: Crea un progetto in OreFattura con le righe di preventivo (a ore, a giornata, a canone mensile o annuale, a corpo) basate sullo storico dei prezzi, e aggiunge attività al catalogo se mancano. Usare quando l'utente chiede di aprire, creare o impostare un progetto, preparare un preventivo, aggiungere righe o attività, o modificare date, stato e righe di un progetto.
---

# Nuovo progetto

Richiede il permesso Gestione progetti. Inizia con `get_workspace_context`. In OreFattura si dice «progetto», mai «commessa».

## 1. Cliente

Trova il `client_id` con `search` o `get_clients`. Se il cliente non esiste, usa prima la skill *nuovo-cliente*.

## 2. Righe di preventivo

1. Per ogni lavoro descritto cerca l'attività nel catalogo con `get_activities`.
2. Se manca, proponila con `propose_activity` (nome, tipo di fatturazione, prezzo) e dillo: la riga potrà usarla solo dopo la conferma.
3. Chiama `get_pricing_history` con gli `activity_ids` (max 10). **Non inventare prezzi**: proponi il prezzo citando lo storico («mediana 55 €/h su 12 righe, ultimo progetto Rossi a 60 €/h»). Se lo storico è vuoto usa il prezzo di catalogo e dichiaralo; se manca anche quello, chiedi.
4. Tipo di fatturazione: `hourly`, `daily` (arrotondata alla mezza giornata), `monthly` / `yearly` (canone, con `start_date` e eventuale `end_date`), `fixed` (a corpo). Quantità = ore, giornate, mesi o 1 per il corpo.

## 3. Progetto

Campi: `name`, `client_id`, `status` (1 in corso, default), `start_date`, `deadline`, `notes`, `lines`, `user_ids` (solo se l'utente indica le risorse). Mostra un riepilogo con totale stimato per riga e complessivo, poi `propose_project` una sola volta.

## Modifiche a un progetto esistente

`get_project`, poi `propose_project_update` passando solo ciò che cambia; le righe si tolgono solo con `remove_line_ids`. Movimenti di costo o ricavo extra: `propose_project_transaction`.

Chiudi sempre con il link e ricorda che la proposta va confermata in OreFattura.
