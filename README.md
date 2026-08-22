# SP7KJR · Status radiowy

Lekki, responsywny panel statusu stacji SP7KJR, przeznaczony do publikacji w GitHub Pages. Strona pobiera bieżące dane z publicznego arkusza Google i nie wymaga żadnego serwera ani procesu budowania.

## Dane w arkuszu

W zakresie \`A:B\` arkusza użyj w pierwszej kolumnie nazw pola, a w drugiej jego wartości:

| Pole | Obsługiwane nazwy | Przykład |
| --- | --- | --- |
| Status | \`status\`, \`stan\` | \`ONLINE\`, \`OFFLINE\`, \`QSX (RX/TX)\` |
| Aktualizacja | \`updated\`, \`ostatnia aktualizacja\`, \`aktualizacja\` | \`22.08.2026, 17:46\` |
| Częstotliwość | \`frequency\`, \`częstotliwość\`, \`przemiennik\` | \`145.500 FM\` |
| Sieć | \`network\`, \`sieć\` | \`EchoLink\` |
| QTH | \`qth\`, \`lokalizacja\` | \`Łódź\` |

## Niezawodność i bezpieczeństwo

- pobieranie danych ma limit czasu 12 s i maksymalnie dwie ponowne próby;
- odświeżanie automatyczne odbywa się co minutę, a przycisk pozwala uruchomić je ręcznie;
- ostatnie poprawne dane są zapisywane lokalnie w przeglądarce i zostają wyświetlone, gdy arkusz chwilowo nie odpowiada;
- dane z arkusza są walidowane i zawsze wstawiane przez \`textContent\`, bez interpretowania HTML;
- polityka CSP ogranicza połączenia strony do Google Sheets i blokuje osadzanie aktywnej treści z innych źródeł;
- interfejs ma czytelne statusy połączenia, obsługuje klawiaturę oraz ustawienie ograniczenia animacji systemu.

## Konfiguracja

Identyfikator arkusza jest zapisany w stałej \`SHEET_ID\` w [index.html](index.html). Arkusz musi być opublikowany lub udostępniony do odczytu dla odwiedzających stronę.
