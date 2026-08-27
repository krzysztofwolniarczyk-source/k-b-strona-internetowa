# K&B Meble Na Wymiar — strona

Strona wizytówka pracowni: **`index.html`** plus katalog **`zdjecia/`**. Style i skrypt
siedzą w pliku HTML, nie ma nic do budowania ani instalowania — wystarczy wrzucić
całość na hosting albo do repozytorium z GitHub Pages.

Styl zdjęty z animowanego banera: czerń `#08080A`, złoto `#F5A800`, rdzeń poświaty
`#FCD800`, biel `#F5F5F5`. Kroje: **Jost** (nagłówki, interfejs) i **Space Mono**
(opisy techniczne) — oba z Google Fonts.

## Co jest w katalogu

```
index.html        cała strona
baner.mp4         animowany baner w nagłówku strony
baner.jpg         plansza banera — widoczna, zanim film ruszy
baner.gif         ta sama animacja dla przeglądarek bez obsługi wideo
og-image.jpg      miniatura do podglądu linku na Facebooku
zdjecia/          16 realizacji, każda w dwóch wersjach
  01.jpg          dłuższy bok 1800 px — otwiera się po kliknięciu
  01-mini.jpg     dłuższy bok  900 px — widoczne w siatce
  _meta.json      opisy i wymiary (plik roboczy, nie jest używany przez stronę)
```

## Uruchomienie

Otwórz `index.html` w przeglądarce. Tyle.

## Baner i pulsowanie światła

Nagłówek strony to animowany baner odtwarzany w pętli: `baner.mp4`, bez dźwięku,
z planszą `baner.jpg` na czas wczytywania i `baner.gif` jako zapasem dla przeglądarek
bez obsługi wideo. Krawędzie są wygaszone w tło, więc grafika świeci, zamiast stać
w ramce. Szerokość ograniczają trzy wartości naraz — `min(1040px, 100%, 89vh)` —
dzięki czemu proporcja 16:9 trzyma się na każdym ekranie, a baner nigdy nie zasłania
nagłówka ani przycisków.

Podmiana banera: nadpisz trzy pliki tą samą nazwą. Planszę wygodnie zrobić
z najjaśniejszej klatki animacji.

Światłem pulsuje cała strona — znak firmowy, nadpisy sekcji, główne przyciski, narożniki
tabliczek, ikony procesu i świetlny szew na styku sekcji. Każdy element ma inny czas
cyklu (od 4,2 do 7,5 s), żeby nic nie migotało jednym rytmem. Wszystko siedzi w czterech
klatkach `@keyframes puls-*` na początku arkusza stylów — zmiana tempa albo siły poświaty
to jedno miejsce w kodzie.

Kto ma w systemie włączone ograniczenie ruchu (`prefers-reduced-motion`), dostaje stronę
bez pulsowania i z zatrzymanym banerem, ale poświata zostaje — strona dalej świeci,
tylko nieruchomo.

## Galeria

Siatka jest **murowana** (`columns`), nie kafelkowa — zdjęcia zachowują pełny kadr
zamiast być przycinane do kwadratu. Przy szerokim oknie idą w 4 kolumnach, potem 3, a na
telefonie 2. Kolejność w kodzie przeplata kadry pionowe z poziomymi, żeby kolumny
kończyły się na podobnej wysokości — dokładając zdjęcia warto trzymać ten rytm.

Kafle są bez podpisów — sama fotografia. Kliknięcie otwiera powiększenie ze strzałkami,
obsługą klawiatury (← → Esc), licznikiem „3 / 16" i opisem realizacji pod zdjęciem.

### Dodanie kolejnego zdjęcia

1. Przygotuj dwa pliki w `zdjecia/`: `17.jpg` (dłuższy bok 1800 px) i `17-mini.jpg`
   (dłuższy bok 900 px). Zdjęcia z telefonu warto przepuścić przez kompresję —
   te w katalogu mają po 90–230 kB i 25–60 kB.
2. Dopisz kafel na końcu listy w sekcji `#realizacje`:

```html
<a class="gal-item" href="zdjecia/17.jpg" data-caption="Kuchnia w Jezioranach — fronty lakierowane">
  <img src="zdjecia/17-mini.jpg" alt="Kuchnia w Jezioranach — realizacja K&B Meble Na Wymiar"
       width="900" height="405" loading="lazy" decoding="async">
</a>
```

- `width` i `height` to wymiary **miniatury** — trzymają układ, zanim zdjęcie się wczyta.
- `data-caption` to podpis widoczny **w powiększeniu**, `alt` opis dla wyszukiwarki
  i czytników ekranu. Na kaflach w siatce nie ma żadnych podpisów — same zdjęcia.
- Powiększenie podłącza się samo: skrypt zbiera wszystkie `.gal-item` z atrybutem `href`.
- Bez JavaScriptu zdjęcie po prostu otworzy się w nowej karcie.

## Numery telefonów

| Kto | Numer | Gdzie występuje |
|---|---|---|
| Bartłomiej | +48 888 460 721 | pasek górny, hero, menu mobilne, tabliczka, karta kontaktowa, pasek CTA, stopka, pasek na telefonie, dane strukturalne |
| Krzysztof | +48 510 864 253 | te same miejsca + WhatsApp |

Najprościej szukać ciągów `888460721` i `510864253`. WhatsApp prowadzi na numer Krzysztofa
(`wa.me/48510864253`) — jeśli ma prowadzić gdzie indziej, podmień w trzech miejscach.

## Co podmienić przy przenosinach na własną domenę

W sekcji `<head>` trzy miejsca trzymają adres strony:

| Znacznik | Do czego służy |
|---|---|
| `<link rel="canonical">` | adres właściwy dla wyszukiwarek |
| `og:url` | adres w podglądzie linku na Facebooku |
| `og:image` / `twitter:image` | miniatura podglądu (`og-image.jpg` obok pliku) |

W bloku `application/ld+json` na końcu strony jest jeszcze `"url"` — ten sam adres.

## Co strona robi sama

- **Godziny** — dzisiejszy dzień podświetla się sam, a pod tabelą stoi status
  „otwarte / zamknięte" liczony z zegara odwiedzającego. Godziny zmienia się
  w dwóch miejscach: w tabeli HTML i w stałej `GODZINY` w skrypcie.
- **Menu na telefonie** — pełnoekranowe, zamyka się po kliknięciu w link i klawiszem Esc.
- **Nagłówek** — przykleja się do góry i dostaje cień po przewinięciu.
- **Sekcje** — pojawiają się przy przewijaniu, ale bez JavaScriptu są normalnie
  widoczne (animacja włącza się dopiero, gdy skrypt wystartuje).
- **Rok w stopce** — podstawia się sam.

## Dostępność i SEO

- Pełny zestaw `<meta>`: opis, `canonical`, Open Graph i Twitter Card.
- Dane strukturalne `LocalBusiness` (JSON-LD) — oba telefony jako `contactPoint`,
  godziny otwarcia, obszar działania i profil na Facebooku.
- Link „Przejdź do treści", widoczne obramowania fokusa, `aria-*` na menu i powiększeniu,
  obsługa `prefers-reduced-motion`, arkusz do druku.
- Brak poziomego przewijania od 320 px wzwyż (sprawdzone przy 360, 390, 768, 900 i 1440 px).
- Zdjęcia ładują się leniwie i mają podane wymiary, więc układ nie skacze przy wczytywaniu.
