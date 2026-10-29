# OreFattura per Claude

OreFattura è il software italiano che porta le ore lavorate fino alla fattura elettronica trasmessa al Sistema di Interscambio (SdI). Questo plugin collega Claude al tuo workspace OreFattura e aggiunge flussi di lavoro pronti per freelance, partite IVA e piccoli team: registrare le ore, chiudere il mese, capire cosa fatturare, tenere d'occhio budget e redditività dei progetti, creare clienti, progetti, attività e task.

## Cosa contiene

- **Connettore OreFattura** (server MCP remoto, circa 50 strumenti). Al primo uso Claude apre il browser su OreFattura: entri con le tue credenziali, leggi cosa potrà fare l'assistente e premi «Autorizza». Nessuna chiave da copiare.
- **Skill**:
  - *Registra ore*: da una frase («ieri 3 ore sul sito Rossi») o dal calendario a righe di timesheet, senza superare le ore della giornata.
  - *Chiudi il mese*: ore mancanti, proposte in attesa, clienti da fatturare, scadenze, fatturato e stima indicativa delle tasse.
  - *Verbale riunione*: dagli appunti di una riunione a ore, nota di progetto e task.
  - *Stato progetti*: budget consumato, redditività, task aperti, statistiche di ore e fatturato.
  - *Nuovo cliente*: anagrafica completa per la fattura elettronica (partita IVA, codice destinatario o PEC).
  - *Nuovo progetto*: progetto con righe di preventivo basate sullo storico dei tuoi prezzi.
  - *Gestisci task*: crea, sposta, commenta e aggiorna i task della bacheca.

## Requisiti

- Un account OreFattura con il server MCP acceso (Impostazioni → Assistente MCP). I membri del team hanno bisogno del permesso «Assistente MCP».
- Per registrare le ore dal calendario serve anche un connettore calendario in Claude (ad esempio Google Calendar).

## Come gestisce i dati

- Il plugin non esegue codice sul tuo computer: contiene solo istruzioni per Claude e l'indirizzo del server MCP di OreFattura.
- Claude legge dal tuo workspace solo ciò che puoi vedere tu, con i tuoi permessi, tramite il server MCP di OreFattura. Nessun dato viene inviato ad altri servizi dal plugin.
- **Ogni scrittura è una proposta**: ore, task, clienti e progetti compaiono in OreFattura con le etichette «AI» e «Da confermare» e diventano definitivi solo quando li confermi tu.
- L'assistente non emette fatture, non invia nulla allo SdI né ai clienti, non registra incassi e non modifica team o impostazioni.
- Le stime fiscali sono indicative: vanno verificate con il commercialista.
- Puoi revocare il collegamento in qualsiasi momento da OreFattura → menu del profilo → Connessioni.

## Supporto

Simone Alessandrini (AraLith) — dalla pagina di supporto di OreFattura.

## Privacy

[Informativa privacy di OreFattura](https://orefattura.aralith.cloud/legale/privacy)

## Licenza

MIT
