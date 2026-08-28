# Radio-Status V2.4

> Nowoczesny, lekki i w pełni statyczny system publikowania statusu krótkofalarskiego w czasie rzeczywistym.

![Version](https://img.shields.io/badge/version[Statusimg.shields.io/badge/status-stable-green
![Platform](https://img.shields.io/badge/platform-Web-orangehttps://img.shields.io/badge/license-Public-lightgreyis treści

- [o-projekcie
- [geneza-projektu
- [najważniejsze-funkcje
- [Architektura systemu](#architekturach
- #technologie
- [budowa-aplikacji
- [System statusów](#system-statusówywy
- #bezpieczeństwo
- [Mechanizmy niezawodności](##wydajność
- [Responsywność](#responsność
- #integracje
- #przykładowe-zastosowania
- [możliwości-rozwoju
- [Autorzy](#autor O projekcie

**Radio-Status V2.4** to webowa aplikacja przeznaczona dla radioamatorów, umożliwiająca publikowanie aktualnego statusu pracy stacji oraz operatora w czasie rzeczywistym.

Projekt został zaprojektowany jako rozwiązanie:

- proste,
- szybkie,
- bezobsługowe,
- niewymagające serwera aplikacyjnego,
- możliwe do uruchomienia na dowolnym hostingu statycznym.

Aplikacja pobiera dane z publicznego arkusza Google Sheets i automatycznie odświeża wyświetlane informacje bez konieczności przeładowywania strony. 【1-79ad73】【2-9954e1】

---

# Geneza projektu

W środowisku krótkofalarskim często pojawia się potrzeba szybkiego przekazania informacji:

- czy operator jest aktywny,
- czy aktualnie prowadzi QSO,
- na jakim przemienniku pracuje,
- w jakiej lokalizacji się znajduje,
- czy nasłuchuje określonej częstotliwości.

Większość istniejących rozwiązań nie została zaprojektowana specjalnie do takiego celu.

Radio-Status powstał jako odpowiedź na te potrzeby.

Założenia od początku były proste:

1. Zero backendu.
2. Zero baz danych.
3. Maksymalna niezawodność.
4. Możliwość aktualizacji statusu przez telefon.
5. Łatwa instalacja dla każdego krótkofalowca.

---

# Najważniejsze funkcje

## Aktualizacja w czasie rzeczywistym

System automatycznie pobiera dane z arkusza Google co 60 sekund. 【1-79ad73】

## Publiczny panel statusowy

Każdy użytkownik posiada jeden publiczny panel pokazujący:

- status,
- datę aktualizacji,
- przemiennik lub częstotliwość,
- sieć,
- lokalizację QTH,
- komentarz. 【1-79ad73】【2-9954e1】

## Dynamiczne motywy

Kolorystyka interfejsu automatycznie zmienia się zależnie od rodzaju aktywności operatora. 【1-79ad73】

## Cache lokalny

Ostatnie dane są przechowywane lokalnie w przeglądarce użytkownika. 【1-79ad73】

## Odporność na błędy

Aplikacja posiada:

- mechanizm timeout,
- automatyczne ponawianie połączeń,
- obsługę awarii sieci,
- pracę w trybie danych zapisanych lokalnie. 【1-79ad73】

---

# Architektura systemu

## Model logiczny

```text
Google Sheets
       │
       ▼
Google Visualization API
       │
       ▼
JavaScript Fetch API
       │
       ▼
Parser danych
       │
       ▼
Interfejs użytkownika
```

Cała logika aplikacji wykonywana jest po stronie klienta. 【1-79ad73】

Nie występują:

- Node.js
- PHP
- Python
- MySQL
- PostgreSQL
- Docker
- Backend API

---

# Źródło danych

Aplikacja wykorzystuje publiczny arkusz Google Sheets jako źródło danych operacyjnych.

Przykładowa struktura:

```text
status / notice    OFFLINE
updated            23.08.2026, 16:12
frequency          SR7KU
network            NO NETWORK
qth                Lodz
comment
```

Aktualne dane są mapowane do pól aplikacji i przetwarzane przez parser JavaScript. 【2-9954e1】

---

# Technologie

## Frontend

- HTML5
- CSS3
- Vanilla JavaScript ES6+ 【1-79ad73】

## API

- Google Visualization API
- Google Sheets API endpoint (`gviz`) 【1-79ad73】

## Przechowywanie danych

- Google Sheets
- LocalStorage przeglądarki 【1-79ad73】

---

# Budowa aplikacji

## Struktura dokumentu

Aplikacja została zaimplementowana jako pojedynczy plik HTML.

Główne sekcje:

```text
Header
Status Panel
Dashboard
Comments
Contact Section
Abbreviations
QRZ Logbook
Footer
```

Architektura typu Single File Application upraszcza wdrożenie i utrzymanie. 【1-79ad73】

---

# System statusów

## OFFLINE

Operator niedostępny.

Przykład:

```text
OFFLINE
```

Motyw:

```text
Ciemny granat + cyjan
```

---

## QRL / DND

Operator zajęty.

Przykłady:

```text
QRL
DND
```

Motyw:

```text
Ciemna czerwień
```

---

## SCAN / QSX

Aktywna praca radiowa.

Przykłady:

```text
SCAN
QSX
```

Motyw:

```text
ON AIR
Zielony
```

---

## MOBILE / PORTABLE

Praca terenowa.

Przykłady:

```text
MOBILE
PORTABLE
```

Motyw:

```text
Pomarańczowy
```

---

## QAZ / ALERT

Status alarmowy.

Przykłady:

```text
QAZ
ALERT
```

Motyw:

```text
Czerwony alarmowy
```

Mechanizm automatycznie przypisuje odpowiednią klasę CSS na podstawie treści statusu. 【1-79ad73】

---

# Dynamiczne motywy

System motywów stanowi jeden z najbardziej zaawansowanych elementów projektu.

Zmianie podlegają:

- gradienty tła,
- kolory paneli,
- animacje,
- efekty świetlne,
- kolory przycisków,
- wskaźniki aktywności. 【1-79ad73】

Dla statusów ON AIR oraz ALERT dodatkowo aktywowane są animacje pulsowania. 【1-79ad73】

---

# Bezpieczeństwo

Aplikacja stosuje politykę bezpieczeństwa Content Security Policy (CSP).

Dopuszczone są wyłącznie:

```text
self
docs.google.com
logbook.qrz.com
```

Ogranicza to ryzyko:

- XSS,
- nieautoryzowanych połączeń,
- złośliwych skryptów zewnętrznych. 【1-79ad73】

---

# Mechanizmy niezawodności

## Timeout

Każde połączenie posiada limit:

```javascript
12 sekund
```

【1-79ad73】

---

## Retry

System automatycznie ponawia nieudane pobranie danych.

```javascript
MAX_RETRIES = 2
```

【1-79ad73】

---

## Cache

Dane przechowywane są lokalnie pod kluczem:

```javascript
sp7kjr-radio-status-v2
```

W przypadku awarii źródła aplikacja wyświetla ostatnie zapisane dane. 【1-79ad73】

---

# Wydajność

Radio-Status V2.4 został zaprojektowany zgodnie z filozofią:

> "Maksimum funkcjonalności przy minimalnej liczbie zależności."

Cechy:

- pojedynczy plik HTML,
- brak bibliotek JavaScript,
- brak frameworków,
- brak backendu,
- bardzo niski transfer danych. 【1-79ad73】

Dzięki temu aplikacja może działać nawet na bardzo prostym hostingu statycznym.

---

# Responsywność

Interfejs automatycznie dostosowuje się do wielkości ekranu.

Obsługiwane urządzenia:

- smartfony,
- tablety,
- laptopy,
- komputery stacjonarne. 【1-79ad73】

Próg mobilny:

```css
560px
```

【1-79ad73】

---

# Dostępność

Projekt został przygotowany zgodnie z nowoczesnymi zasadami dostępności.

Wykorzystane elementy:

```html
aria-label
aria-live
role="status"
role="alert"
```

【1-79ad73】

Korzyści:

- lepsza obsługa czytników ekranowych,
- poprawiona nawigacja,
- zgodność z wymaganiami dostępności.

---

# Integracje

## QRZ.com

Profil stacji:

```text
https://www.qrz.com/db/SP7KJR
```

【1-79ad73】

---

## QRZ Logbook

Osadzony dziennik łączności:

```text
https://logbook.qrz.com/lbstat/SP7KJR/
```

【1-79ad73】

---

## Google Sheets

Źródło statusów:

```text
Google Sheets
↓
Visualization API
↓
Radio-Status
```

【1-79ad73】【2-9954e1】

---

# Przykładowe zastosowania

## Indywidualny krótkofalowiec

Publikowanie własnego statusu radiowego.

## Klub krótkofalarski

Informacja o aktywności klubowej stacji.

## Stacja okolicznościowa

Bieżąca informacja dla operatorów i korespondentów.

## Łączność kryzysowa

Status dyżurów i dostępności operatorów.

## Aktywacje terenowe

SOTA, POTA, WWFF, praca mobilna oraz portable.

---

# Możliwości rozwoju

Przyszłe wersje mogą zawierać:

- obsługę wielu stacji,
- historię statusów,
- integrację APRS,
- integrację BrandMeister,
- integrację DMR,
- automatyczne wykrywanie aktywności,
- API REST,
- statystyki aktywności,
- mapę lokalizacji,
- powiadomienia push,
- eksport danych.

---

# Ocena techniczna

| Obszar | Ocena |
|----------|----------|
| Architektura | 9/10 |
| Wydajność | 10/10 |
| Prostota wdrożenia | 10/10 |
| Niezawodność | 9/10 |
| Czytelność kodu | 9/10 |
| Użyteczność | 10/10 |
| Skalowalność | 7/10 |

---

# Autorzy

## Twórca projektu

**Kacper Juszko**

Pomysł, projekt architektury, implementacja oraz rozwój aplikacji Radio-Status.

## Wsparcie AI

Projekt został opracowany przy wykorzystaniu nowoczesnych narzędzi sztucznej inteligencji:

- ChatGPT
- OpenAI Codex
- GitHub Copilot / Gemini

które wspierały proces:

- projektowania architektury,
- implementacji kodu,
- optymalizacji interfejsu użytkownika,
- dokumentowania projektu.

---

# Podsumowanie

Radio-Status V2.4 jest lekkim, wydajnym i nowoczesnym systemem publikowania statusu krótkofalarskiego działającym całkowicie po stronie klienta. Projekt wykorzystuje Google Sheets jako źródło danych, automatyczne odświeżanie, lokalną pamięć podręczną oraz dynamiczne motywy wizualne zależne od aktywności operatora. Dzięki całkowitemu brakowi backendu rozwiązanie jest wyjątkowo łatwe do wdrożenia i utrzymania, zachowując jednocześnie wysoką funkcjonalność oraz atrakcyjny wygląd. 【1-79ad73】【2-9954e1】
