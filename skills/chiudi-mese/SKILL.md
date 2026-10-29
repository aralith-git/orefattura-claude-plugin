---
name: chiudi-mese
description: Guida la chiusura del mese in OreFattura - ore mancanti, proposte ancora da confermare, clienti con qualcosa da fatturare, scadenze, fatturato e stima indicativa delle tasse. Usare quando l'utente chiede di chiudere il mese, cosa deve fatturare, chi deve fatturare, a che punto è con il mese, o un riepilogo di fine mese o trimestre.
---

# Chiudi il mese

Produce un controllo di fine mese in un'unica risposta ordinata. Non emette fatture: indica cosa fatturare e dà i link per comporre le fatture in OreFattura.

## Periodo

Chiama `get_workspace_context`. Di default il periodo è il **mese scorso** se oggi è nei primi 10 giorni del mese, altrimenti il **mese in corso**; dichiara all'inizio quale periodo hai scelto. Se l'utente indica un periodo, usa quello.

## Controlli, in quest'ordine

1. **Ore complete.** `get_week_summary` per ogni settimana del periodo: elenca i giorni lavorativi sotto le ore attese, con quante ore mancano. Non riempirli da solo: offri di farlo con la skill *registra-ore*.
2. **Proposte in attesa.** `get_proposals` (status pending): le ore proposte e non confermate non entrano in fattura. Elencale con il link.
3. **Budget.** `get_budget_alerts` per le righe vicine o oltre la stima: da segnalare al cliente prima di fatturare.
4. **Cosa fatturare** (solo con permesso Contabilità). `get_clients`, poi `get_billable` per ogni cliente attivo nel periodo. Tabella: cliente, ore/giornate, righe a corpo o canone, rinnovi, totale, ore AI ancora da confermare, link `compose_url`. Salta i clienti a zero.
5. **Report attività.** Per i clienti che ricevono il report delle ore, offri `propose_report_send`; ricorda che parte solo quando l'utente clicca «Conferma e invia» nella pagina Report.
6. **Scadenze.** `get_deadlines` (30 giorni): rate scadute per prime, poi in arrivo, fatture fornitori, rinnovi.
7. **Numeri.** `get_revenue_stats` per il periodo e l'anno; `get_tax_estimate` per l'anno. Riporta sempre le `assumptions` della stima e scrivi che è indicativa e va verificata con il commercialista.

Senza permesso Contabilità i punti 4, 6 e 7 non sono disponibili: dillo una volta e prosegui.

## Formato della risposta

- In testa, tre righe: periodo, totale da fatturare, cose da fare prima di emettere.
- Poi una sezione per punto, solo se ha contenuto.
- Chiudi con la prossima azione concreta (es. «Conferma le 6 righe AI, poi apri la composizione per Rossi Srl»).

Usa i tool statistici, mai somme fatte a mano sulle righe. Non confrontare né valutare le persone del team.
