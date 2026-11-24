# Filmy poklatkowe z kamery all-sky

Repozytorium zawiera filmy poklatkowe zarejestrowane systemem opisanym w pracy dyplomowej inżynierskiej
**„System monitorowania nieba sterowany komputerem Raspberry Pi”**
(Politechnika Gdańska, Wydział Elektroniki, Telekomunikacji i Informatyki, 2026).
Filmy omówiono w podrozdziale 7.5.1 pracy.

## System

- komputer Raspberry Pi 4,
- kamera Raspberry Pi High Quality Camera z matrycą Sony IMX477,
- obiektyw ArduCam o ogniskowej 3,2 mm i polu widzenia 120°,
- szczelna obudowa z akrylową kopułą,
- oprogramowanie [Allsky](https://github.com/AllskyTeam/allsky) w wersji v2024.12.06_06.

Każdy film utworzył automatycznie skrypt `timelapse.sh` programu Allsky (program ffmpeg) ze wszystkich zdjęć jednej doby,
czyli z dnia i następującej po nim nocy. Rozdzielczość filmów wynosi 2028 × 1520 pikseli, a liczba klatek na sekundę 25.
Wyjątkiem jest film z 12.08.2026, utworzony przy 8 klatkach na sekundę, aby łatwiej było zaobserwować meteory z roju Perseidów.

## Filmy

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

*imię i nazwisko*, Politechnika Gdańska, 2026
