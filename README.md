# Kamera all-sky na Raspberry Pi

Repozytorium dotyczy systemu opisanego w pracy dyplomowej inżynierskiej
**„System monitorowania nieba sterowany komputerem Raspberry Pi”**
(Politechnika Gdańska, Wydział Elektroniki, Telekomunikacji i Informatyki, 2026).
Zawiera spis elementów, krótki opis złożenia i uruchomienia systemu oraz filmy poklatkowe z testów,
omówione w podrozdziale 7.5.1 pracy.

## Spis elementów

| Element | Parametry | Liczba |
|---|---|---:|
| Komputer Raspberry Pi 4 Model B | 4 GB RAM, wbudowane Wi-Fi 2,4/5 GHz | 1 |
| Kamera Raspberry Pi High Quality Camera | matryca Sony IMX477, 4056 × 3040 pikseli, mocowanie CS | 1 |
| Obiektyw ArduCam | mocowanie CS, ogniskowa 3,2 mm, pole widzenia 120° | 1 |
| Taśma do złącza CSI | połączenie kamery z komputerem | 1 |
| Karta pamięci microSD | 128 GB, klasa A2, odczyt do 100 MB/s | 1 |
| Radiator na procesor z dwoma wentylatorami | wentylatory 5 V zasilane z pinów 5 V i GND złącza GPIO | 1 |
| Regulator temperatury W1209 | zasilanie 12 V, czujnik termistorowy, przekaźnik | 1 |
| Wentylator załączany przez W1209 | cyrkulacja powietrza w obudowie | 1 |
| Przetwornica obniżająca napięcie (step-down) | wejście 12/24 V, wyjście 5 V USB-C, do 5 A (25 W) | 1 |
| Moduł rozdzielczy z listwą zaciskową | rozprowadzenie napięcia 12 V w obudowie | 1 |
| Obudowa hermetyczna BOPLA Euromas X-T | 149,9 × 149,9 × 60 mm, tworzywo sztuczne | 1 |
| Przezroczysta kopuła akrylowa | osłona obiektywu | 1 |
| Dławik kablowy PG 11 | szczelność IP68 | 1 |
| Przewód H07RN-F 2 × 1,5 mm² | odporny na warunki atmosferyczne, 10 m | 1 |
| Złącze DC 5,5 × 2,1 mm | rozłączne połączenie zasilacza z przewodem | 1 |
| Zasilacz impulsowy | 230 V AC → 12 V DC, 5 A (60 W), wewnątrz budynku | 1 |

Do montażu potrzebne są też klej do kopuły.

## Złożenie

1. Przykręcić obiektyw do kamery i połączyć kamerę z Raspberry Pi taśmą CSI.
   Taśma ma długość kilkudziesięciu centymetrów, dlatego kamera i komputer są w jednej obudowie.
2. Kopułę przykleić w górnej części obudowy nad obiektywem kamery.
3. Na procesorze zamocować radiator z wentylatorami. Wentylatory podłączyć do pinów 5 V i GND złącza GPIO, pracują one ciągle.
4. Przewód zasilający wprowadzić do obudowy przez dławik PG 11 i podłączyć do listwy zaciskowej.
   Z listwy zasilić przetwornicę 5 V (wyjście USB-C do Raspberry Pi) oraz regulator W1209.
5. Do regulatora W1209 podłączyć czujnik termistorowy i dodatkowy wentylator.
   Na regulatorze ustawić temperaturę załączenia wentylatora.
6. Drugi koniec przewodu podłączyć przez złącze DC do zasilacza 12 V w budynku.
   Przewodem doprowadza się 12 V, a nie 5 V, bo przy tej samej mocy płynie mniejszy prąd i mniejszy jest spadek napięcia na 10 m przewodu.

## Uruchomienie oprogramowania

1. **System operacyjny.** Programem Raspberry Pi Imager wgrać na kartę **64-bitowy** Raspberry Pi OS
   (na wersji 32-bitowej instalacja Allsky się nie ukończyła). W Imagerze ustawić nazwę komputera,
   konto użytkownika i strefę czasową, np. Europe/Warsaw.
2. **Sieć.** Połączyć komputer z siecią Wi-Fi i włączyć SSH. Żeby była widoczna sieć 5 GHz,
   trzeba ustawić kraj sieci bezprzewodowej: `sudo raspi-config` → Localisation Options → WLAN Country → PL.
3. **[Allsky](https://github.com/AllskyTeam/allsky).** Pobrać i zainstalować program:

   ```bash
   cd
   git clone --depth=1 --recursive https://github.com/AllskyTeam/allsky.git
   cd allsky
   ./install.sh
   ```

   W pracy użyto wersji v2024.12.06_06. Instalator pyta m.in. o ustawienia regionalne,
   dodanie pamięci wymiany i współrzędne geograficzne miejsca montażu, od których zależy przełączanie trybu dziennego i nocnego.
4. **WebUI.** Po ponownym uruchomieniu komputera otworzyć w przeglądarce `http://<nazwa-komputera>.local`.
   Domyślny login to `admin`, a hasło `secret` – trzeba je zmienić w zakładce Change Password.
   W zakładce Allsky Settings zatwierdzić ustawienia przyciskiem **Configuration done; start Allsky**.
   Allsky działa jako usługa systemowa i uruchamia się po każdym włączeniu komputera.
5. **Ostrość.** Ustawić ręcznie pierścieniem na kamerze, sprawdzając kolejne zdjęcia w WebUI.
6. **Ustawienia.** W konfiguracji końcowej zmieniono względem wartości domyślnych:

   | Ustawienie w WebUI | Wartość domyślna | Wartość w systemie |
   |---|---|---|
   | Max Auto-Exposure, dzień / noc | 60 s / 60 s | 8 s / 30 s |
   | Delay, dzień / noc | 30 s / 30 s | 1 s / 0,35 s |
   | Gain w nocy (wartość początkowa, przy niej powstają klatki ciemne) | 8 | 10 |
   | Brightness Threshold (obraz śladów gwiazd) | 0,1 | 0,15 |
   | Rozdzielczość filmu poklatkowego (Width, Height) | 4056 × 3040 | 2028 × 1520 |
   | Use Dark Frames | wyłączone | włączone |

   Pozostałe ustawienia, m.in. jasność docelowa (0,5 w dzień, 0,2 w nocy), kąt Słońca −6°,
   progi odrzucania zdjęć 0,1 i 0,9 oraz balans bieli, mają wartości domyślne.
7. **Klatki ciemne.** Całkowicie przykryć kopułę, włączyć Take Dark Frames, zapisać ustawienia i ponownie uruchomić Allsky.
   Po około 30 minutach wyłączyć Take Dark Frames, zdjąć przykrycie i włączyć Use Dark Frames.
   Klatki ciemne zapisują się w folderze `~/allsky/darks`.
8. **Dostęp przez Internet.** Zainstalować usługę Tailscale:

   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh
   sudo tailscale up
   ```

   Drugie polecenie wyświetla odnośnik do zalogowania na konto Tailscale. Na telefonie lub komputerze
   zainstalować aplikację Tailscale i zalogować się na to samo konto. WebUI otwiera się wtedy
   pod adresem komputera w sieci Tailscale, także spoza sieci domowej.

## Filmy

Każdy film utworzył automatycznie skrypt `timelapse.sh` programu Allsky (program ffmpeg) ze wszystkich zdjęć jednej doby,
czyli z dnia i następującej po nim nocy. Rozdzielczość filmów wynosi 2028 × 1520 pikseli, a liczba klatek na sekundę 25.
Wyjątkiem jest film z 12.08.2026, utworzony przy 8 klatkach na sekundę, aby łatwiej było zaobserwować meteory z roju Perseidów.

| Doba | Plik | Liczba klatek | Czas filmu | Rozmiar | Uwagi |
|---|---|---:|---:|---:|---|
| 11.08.2026 | `allsky-20260811.mp4` | 1368 | 54,7 s | 34,4 MB | rejestrację rozpoczęto później z powodu montażu kamery |
| 12.08.2026 | `allsky-20260812.mp4` | 5994 | 749,3 s | 477,8 MB | noc Perseidów 12/13.08, 8 klatek na sekundę |
| 12.08.2026 | `allsky-20260812_NocGwiazd.mp4` | – | – | 25,2 MB | sama noc Perseidów 12/13.08, fragment filmu z 12.08 |
| 13.08.2026 | `allsky-20260813.mp4` | 4422 | 176,9 s | 109,2 MB | |
| 14.08.2026 | `allsky-20260814.mp4` | 2801 | 112,0 s | 69,2 MB | |
| 15.08.2026 | `allsky-20260815.mp4` | 5643 | 225,7 s | 136,1 MB | |
| 16.08.2026 | `allsky-20260816.mp4` | 4552 | 182,1 s | 114,6 MB | pogodna noc 16/17.08, włączono odejmowanie klatek ciemnych |
| 17.08.2026 | `allsky-20260817.mp4` | 3911 | 156,4 s | 90,5 MB | deszcz w nocy 17/18.08 |
| 18.08.2026 | `allsky-20260818.mp4` | 3463 | 138,5 s | 86,9 MB | pogodna noc 18/19.08 |
| 19.08.2026 | `allsky-20260819.mp4` | 3721 | 148,8 s | 88,0 MB | pełne zachmurzenie w nocy 19/20.08 |
| 20.08.2026 | `allsky-20260820.mp4` | 3318 | 132,7 s | 78,9 MB | |

## Pobieranie

Część plików przekracza limit 100 MB, jaki GitHub stawia zwykłym plikom w repozytorium,
dlatego wszystkie filmy udostępniono jako załączniki wydania w zakładce **Releases**:

https://github.com/s193187/Allsky-filmy/releases/tag/v1.0

Filmy można odtworzyć w dowolnym odtwarzaczu obsługującym pliki MP4, np. VLC.

## Autor

Dawid Gawiński, nr albumu 193187, Politechnika Gdańska, 2026
