---
name: nuovo-cliente
description: Crea o aggiorna l'anagrafica di un cliente in OreFattura con i dati che servono alla fattura elettronica (ragione sociale, partita IVA o codice fiscale, codice destinatario SdI o PEC, indirizzo, referenti). Usare quando l'utente chiede di aggiungere, registrare o creare un cliente, incolla i dati di un'azienda o una visura, o vuole correggere i dati di un cliente.
---

# Nuovo cliente

Richiede il permesso Contabilità. Inizia con `get_workspace_context`.

## 1. Evita i doppioni

Cerca prima con `search` o `get_clients` per nome e partita IVA. Se esiste già, passa all'aggiornamento (`get_client`, poi `propose_client_update` con i soli campi che cambiano).

## 2. Raccogli i dati

Prendi i dati dal testo dell'utente; non inventare nulla. Per un cliente italiano servono, per la fattura elettronica:

- `business_name` (o nome e cognome per un privato)
- `vat_number` e/o `tax_code`
- `sdi_code` (codice destinatario, 7 caratteri) **oppure** `pec`; per un privato senza nessuno dei due, il codice `0000000`
- indirizzo completo: `address`, `zip`, `city`, `province` (sigla di 2 lettere), `country` (default IT)

Per un cliente estero imposta `is_foreign: true` e il `country` corretto; il codice destinatario non serve.

Controlli formali prima di proporre: partita IVA italiana di 11 cifre, codice fiscale di 16 caratteri (o 11 cifre per le società), CAP di 5 cifre, provincia di 2 lettere. Se un dato manca o non torna, chiedilo in un'unica domanda con tutti i campi mancanti.

Referenti: `contacts` con tipo (`email`, `phone`, `address`, `other`) e dettagli. Chi riceve i report delle ore si imposta poi nell'app («Invio reportistica»).

## 3. Riepiloga e proponi

Mostra la scheda come apparirà, poi `propose_client` una sola volta. Se risponde `already_present`, mostra il cliente esistente invece di insistere. Chiudi con il link e ricorda la conferma in OreFattura.
