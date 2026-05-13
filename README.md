# Nate Herk Summarizer

Cron GitHub Actions che ogni ora controlla il canale YouTube di Nate Herk (AI Automation), riassume i nuovi video tecnici via Claude Sonnet 4.5 e manda la sintesi strutturata via mail.

Stessa architettura di [lenny-podcast-summarizer](https://github.com/PierpaoloMaggio/lenny-podcast-summarizer) ma con prompt e canale diversi.

## Architettura

```
GitHub Actions cron (orario)
  → fetch RSS canale YouTube Nate Herk
  → diff vs state.json
  → per ogni nuovo videoId non Short:
      Apify transcript → Claude Sonnet 4.5 → SMTP Gmail
  → commit state.json aggiornato
```

## Struttura del riassunto (5 sezioni)

1. **Argomento principale** — una frase
2. **Concetti chiave o novità introdotte** — 4-8 punti
3. **Step tecnici o tutorial** — obiettivo demo + comandi/config/funzionalità + dettagli (se presenti)
4. **Strumenti e funzionalità menzionati** — nome + spiegazione breve
5. **Vantaggi pratici** — 2-4 frasi sui benefici per il workflow

## Secrets richiesti (Settings → Secrets and variables → Actions)

| Nome | Valore |
|---|---|
| `APIFY_TOKEN` | Apify API token |
| `OPENROUTER_KEY` | OpenRouter API key (~73 char, prefisso `sk-or-v1-`) |
| `GMAIL_USER` | `pierpaolo.maggio84@gmail.com` |
| `GMAIL_APP_PASSWORD` | App password Gmail 16 char SENZA spazi |

## Primo avvio

1. Push del repo con i secrets configurati.
2. Actions → "Nate Herk Summarizer" → Run workflow (manuale).
3. Prima run = seed (registra videoId attuali senza mandare mail). Dalla seconda processa solo i nuovi.

## Costo

~$0.10/video con Sonnet 4.5 (input dominato dal transcript, output ~1.5k token). Video di Nate sono di solito 10-30 min → transcript ~10-25k char → ~$0.05-0.10/mail.

A regime (~3-5 video/settimana): **~$1-2/mese**.
