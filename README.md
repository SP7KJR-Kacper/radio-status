# SP7KJR · Radio Status

A lightweight, responsive status dashboard for the SP7KJR amateur radio station. It is designed for GitHub Pages and reads the current information from a public Google Sheet—no server or build step required.

## Sheet data

Use column \`A\` for field names and column \`B\` for their values:

| Field | Supported names | Example |
| --- | --- | --- |
| Status | \`status\`, \`stan\` | \`ONLINE\`, \`OFFLINE\`, \`QSX (RX/TX)\` |
| Last updated | \`updated\`, \`ostatnia aktualizacja\`, \`aktualizacja\` | \`22/08/2026, 17:46\` |
| Frequency | \`frequency\`, \`częstotliwość\`, \`przemiennik\` | \`145.500 FM\` |
| Network | \`network\`, \`sieć\` | \`EchoLink\` |
| QTH | \`qth\`, \`lokalizacja\` | \`Łódź\` |

The dashboard interface is in English. Polish sheet field aliases remain supported for backward compatibility.

## Reliability and security

- Data requests time out after 12 seconds and are retried up to twice.
- The page refreshes automatically every minute; visitors can also refresh it manually.
- The last valid status is stored locally in the browser and shown if the sheet is temporarily unavailable.
- Sheet values are validated and rendered with \`textContent\`; they are never interpreted as HTML.
- The Content Security Policy limits network connections to Google Sheets and blocks active content from other sources.
- The interface provides clear connection states, keyboard-accessible controls, and respects the system reduced-motion preference.

## Configuration

The sheet identifier is stored in the \`SHEET_ID\` constant in [index.html](index.html). The sheet must be published or shared for public read access.
