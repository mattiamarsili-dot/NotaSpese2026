# Spese Lavoro (NotaSpese2026) — istruzioni per Claude

Web app statica, un solo file `index.html` (Tailwind + Lucide da CDN). Rispondi in italiano.
Dati delle spese in `localStorage` (`spese_lavoro_data`), invio a Google Sheets tramite Apps Script
(`GOOGLE_SCRIPT_URL`), download CSV del mese.

## Schede
- NUOVA: inserimento spesa + "Testo libero ✨" (Gemini compila il modulo).
- ELENCO, RIEPILOGO (totali del mese per categoria e pagamento).
- CONTROLLO (ottobre 2026): confronto scontrini fisici ↔ estratto carta.

## Gemini
- Account Google gratuito (livello free di AI Studio), niente piano a pagamento.
- Chiave e modello si impostano dall'app (CONTROLLO › Impostazioni IA) e restano nel browser
  (`gemini_api_key`, `gemini_model`, predefinito `gemini-flash-latest`). Mai scrivere la chiave nel codice.
- Il piano Claude Pro di Mattia non include l'API: Claude si usa solo come ripiego manuale in chat
  (elenco scontrini in CSV → pulsante "Importa CSV scontrini", colonne data;importo;esercente).

## Scheda CONTROLLO
- Foto scontrini (8–12 per foto) → Gemini restituisce per ognuno data, importo, esercente, categoria
  (pedaggio/cibo/carburante/parcheggio/altro), pagamento, leggibile. Salvati in `scontrini_letti`.
- Estratto carta UniCredit in **Excel** (il PDF non si legge), SheetJS caricato da cdnjs solo quando serve.
  Colonne riconosciute in automatico, confermabili a mano.
- All'IA vanno solo le foto: Excel e abbinamento restano nel browser (zero token).
- Abbinamento: importo uguale al centesimo, data entro ±3 giorni, bonus per nome esercente e categoria.
  Risultato: abbinati / in banca senza scontrino / scontrini non in banca; filtri Pedaggi e Cibo.
- Pedaggi pagati con carta a ogni casello (niente Telepass).

## Da fare (in sospeso)
- Mattia deve creare la chiave su aistudio.google.com/apikey dal telefono e incollarla nell'app.
- Provare con foto reali e con un Excel UniCredit vero: verificare segno degli importi (positivi/negativi,
  una o due colonne) e nomi delle colonne; ritoccare `pickColumn`/`getBankMovements` se serve.
- Idea non fatta: pulsante per aggiungere gli scontrini abbinati all'elenco spese.

## Prove
Playwright in locale con CDN intercettate (Tailwind, Lucide, cdnjs → `xlsx` da npm) e risposta Gemini simulata.
