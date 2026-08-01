# CLAUDE.md — jak pracujemy w tym repo

Ten plik czytasz najpierw. Opisuje konwencje projektu i zasady współpracy z AI.
Prywatne notatki (jeśli istnieją) siedzą w `CLAUDE.local.md` — poza gitem.

@CLAUDE.local.md

## Czym jest ten projekt

Eval harness mierzący, jak **sposób organizacji wiedzy** wpływa na wierność faktom,
halucynacje i uczciwe „nie wiem" w treściach generowanych przez LLM. Część praktyczna
pracy magisterskiej, prowadzona metodologią Design Science Research — artefakt jest
przedmiotem badania, nie tylko narzędziem.

Pełne uzasadnienie osi, drabina ablacyjna i plan realizacji leżą **poza tym repo**,
w prywatnej bibliotece „Profil" (`cele/projekty-aktywne/magisterka/`).

## ⛔ Zasada nadrzędna: kod piszę SAM

**Nie generujesz kodu Pythona.** To nie jest preferencja stylistyczna — to cel projektu.

Ta praca jest jednocześnie treningiem Pythona, bo „Python obroniony na rozmowie bez AI"
jest kompetencją blokującą całą ścieżkę zawodową. Kod wygenerowany przez AI nie realizuje
tego celu, nawet jeśli działa.

**Co wolno Ci robić:**
- tłumaczyć pojęcia, biblioteki, komunikaty błędów
- robić review napisanego przeze mnie kodu i wskazywać problemy
- podpowiadać kierunek („tu przyda się generator zamiast listy") bez pisania implementacji
- pisać i edytować pliki **nie-`.py`**: konfigurację, dokumentację, YAML z danymi, CI

**Czego nie wolno:**
- pisać ani edytować plików `.py` — nawet „na szybko", „dla przykładu" czy „bo to trywialne"
- podawać gotowych bloków kodu do skopiowania

Jeśli utknąłem, tłumacz mechanizm i pokaż analogiczny przykład **z dokumentacji**, nie
gotowe rozwiązanie mojego problemu.

## Architektura — co jest stałe

- **Jeden wspólny interfejs dla wszystkich ramion** (`src/kbeval/systems/base.py`):
  ramię dostaje zadanie, oddaje odpowiedź + metadane (tokeny, koszt). Uczciwość porównania
  ma wynikać ze struktury kodu, nie z deklaracji.
- **Jedno ramię = jeden plik** w `systems/`. Dołożenie ramienia nie może wymagać zmian
  w harnessie.
- **Ocena nie wie, które ramię ocenia.** Warstwa `eval/` operuje na odpowiedziach, nie na systemach.
- **`data/` jest danymi, nie kodem.** Marki, kartoteka faktów, pułapki i zadania w YAML.
- **`results/` jest wersjonowane.** Każdy przebieg zostaje w historii — to zapis postępu.

## Konwencje

- **Język:** kod, komentarze, nazwy plików, README i commity — **po angielsku**.
  Repo jest publiczne i czytane też poza Polską.
- **Commity:** `typ: opis w trybie rozkazującym`, ciało wyjaśnia DLACZEGO. Szczegóły niżej.
- **Testy:** każdy moduł w `eval/` ma test. Logika oceny bez testu jest niewiarygodna,
  a to jest praca o wiarygodności pomiaru.
- **Sekrety:** wyłącznie przez `.env`. Nigdy w kodzie, nigdy w YAML-u, nigdy w promptach.
- **Dane osobowe:** marki są syntetyczne. Realne dane klientów nie wchodzą tu w żadnej formie.

## Format commitów

```
typ: opis w trybie rozkazującym, do ~50 znaków

Ciało: dlaczego ta zmiana, nie co zmieniła (co widać w diffie).
Zawijaj ~72 znaki.
```

Typy: `feat` (nowa funkcja) · `fix` (naprawa) · `docs` · `test` · `refactor` ·
`chore` (konfiguracja, zależności) · `ci` · `data` (marki, golden dataset) ·
`exp` (przebieg eksperymentu i wyniki)

Jeden commit = jedna logiczna zmiana. Nie mieszaj refaktoru z nową funkcją.

## Rytm pracy

**Domykaj publicznie co dwa tygodnie**, nie raz na końcu: commit + aktualizacja sekcji
Results/Roadmap w README + krótka notka „co teraz działa". Mechanizm przeciw znanej
słabości w finalizowaniu długich zadań — domykanie ma się zdarzyć osiem razy po trochu,
a nie raz w skali, której historycznie nie dowożę.

## Zakres — czego tu NIE ma

Świadomie poza zakresem do czasu domknięcia łańcucha oceny end-to-end:
wariant RAG na Azure, pełna kombinatoryka warstw, więcej niż cztery ramiona,
więcej niż jedna marka. Nie proponuj ich; jeśli uznasz, że coś z tej listy jest
konieczne, powiedz to wprost i uzasadnij, zamiast dokładać po cichu.
