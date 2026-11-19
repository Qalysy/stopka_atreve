# Generator stopki mailowej ATREVE

Statyczna strona (bez logowania i bazy danych). Pracownik wpisuje dane, widzi podgląd i kopiuje stopkę do Gmaila lub Outlooka. Dane pracowników nie są nigdzie zapisywane.

```
stopka/
├── index.html    generator
├── config.json   listy salonów i social mediów (edytuje administrator)
└── assets/       logo, ikony social, pasek znaku wodnego (PNG)
```

## Wgranie na WordPress (administrator)

1. Wgraj cały folder `stopka/` na serwer do katalogu `/stopka/` (FTP lub menedżer plików hostingu), tak by generator działał pod `https://www.atreve.pl/stopka/`.
2. W `index.html` sprawdź stałą `CONFIG.assetBase`. Ma wskazywać katalog z grafikami, z `https` i ukośnikiem na końcu, np. `https://www.atreve.pl/stopka/assets/`. To ten adres trafia do maila, więc grafiki muszą być publicznie dostępne.
3. Otwórz stronę i sprawdź, że nie widać żółtych ostrzeżeń, a logo i ikony się wyświetlają.

Strona musi być otwierana przez `https://` lub serwer HTTP. Otwarcie pliku z dysku (`file://`) nie wczyta `config.json`.

## Zmiana list (`config.json`)

Pracownicy nie edytują tego pliku. Po zmianie wgraj go na serwer i odśwież stronę (Ctrl+F5).

- **Kolejność** wyświetlania to rosnące `order`.
- **Nowy salon:** dopisz do `salons` obiekt `{ "name": "Poznań", "slug": "poznan", "url": "", "order": 6 }`. `slug` bez polskich znaków i spacji (trafia do UTM jako `salon-poznan`). Puste `url` oznacza zwykłą nazwę, `url` zaczynające się od `https://` (np. wizytówka Google Maps) robi z nazwy link.
- **Nowy profil social:** dopisz do `socials` obiekt `{ "name": "YouTube", "slug": "youtube", "href": "https://www.youtube.com/@atreve", "icon": "ico-youtube.png", "order": 5 }`. Profil bez adresu `https` w `href` jest pomijany.
- **Nowa ikona:** wrzuć PNG 36 × 36 px do `assets/` i wpisz jego nazwę w polu `icon`. Może to być też adres `https` lub data URI. W `assets/` jest już `ico-youtube.png`.
- Uważaj na przecinki i cudzysłowy, błędny JSON wywoła ostrzeżenie na stronie.

## Dla pracowników

1. Wpisz imię i nazwisko, stanowisko, telefon i e-mail (pola z *). Sam początek adresu (`anna.kowalska`) wystarczy, domena `@atreve.pl` zostanie dopisana.
2. Sprawdź podgląd (przełącznik Komputer / Telefon).
3. Kliknij „Kopiuj stopkę”.
4. **Gmail:** Ustawienia → Zobacz wszystkie ustawienia → Ogólne → Podpis, wklej i zapisz.
   **Outlook:** Ustawienia → Poczta → Redagowanie i odpowiadanie → Podpis, wklej i zapisz.
5. Wyślij test do siebie i sprawdź, czy logo się wyświetla.

## Testy

Automatyczne (headless Chromium, wykonane):
- brak poziomego przewijania przy 390 px i 1280 px (`scrollWidth` = szerokość okna), także w widoku „Telefon”,
- każdy `img` w podglądzie ma `naturalWidth > 0`,
- każdy link (poza `mailto:`) ma `utm_source`, `utm_medium`, `utm_campaign`, `utm_content` (np. `anna-kowalska`) i `utm_term`,
- ostrzeżenie przy pustym `assetBase` i przy błędzie wczytania `config.json`.

Ręczne (do wykonania przez administratora, po wgraniu na serwer). Wklej stopkę, wyślij test i porównaj z podglądem:
- Gmail w przeglądarce,
- Gmail w aplikacji na Androidzie,
- iPhone Mail,
- Outlook desktop.

Każdą rozbieżność zgłaszaj jako błąd.

## Ograniczenia

- W mailu jest czcionka Arial (kroju firmowego klienty pocztowe nie wczytają).
- Pasek ze znakiem wodnym to obrazek.
- Stopka w mailu nie zawiera JavaScriptu.
