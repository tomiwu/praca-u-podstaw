# PRD: Praca u podstaw

## Opis problemu

Polscy twórcy open source nie mają wspólnego, oddolnego miejsca do tworzenia projektów, dzielenia się wiedzą i wzajemnej pomocy. Nie ma polskiego odpowiednika niemieckiego openCode.de ani Codeberg. Kluczowe narzędzia cyfrowe (komunikacja, kryptografia, infrastruktura) nie mają polskich, jawnych i audytowalnych alternatyw.

## Rozwiązanie

Oddolna platforma, na której:
- projekty open source są udostępniane P2P w IPFS,
- dowolny podmiot zgłasza zadania z budżetem (pieniądze zablokowane w kontrakcie albo moc obliczeniowa),
- wykonawcy zdobywają punkty i rangę, a wyższa ranga daje prawo do prowadzenia zespołów i moderacji,
- spotkania, nagrania i streaming ekranu działają w istniejącym narzędziu Tomasza,
- asystent AI (Bielik lub inny LLM) działa na puli węzłów ochotników.

„Bezpieczeństwo państwa” oznacza tu jawne projekty wzmacniające cyfrową niezależność, a nie projekty wojskowe czy tajne. Start jest oddolny, bez współpracy z państwem. Fundacja powstanie później.

## Historyjki użytkownika

1. Jako twórca chcę opublikować projekt w IPFS, żeby był dostępny bez centralnego serwera.
2. Jako twórca chcę szyfrować prywatne pliki przed publikacją, żeby nikt niepowołany ich nie odczytał.
3. Jako czytelnik chcę przeglądać projekty publicznie, żeby korzystać z wiedzy innych.
4. Jako zleceniodawca chcę zgłosić zadanie z opisem i kryteriami odbioru, żeby znaleźć wykonawców.
5. Jako zleceniodawca chcę zablokować pieniądze w kontrakcie, żeby wykonawca miał gwarancję zapłaty.
6. Jako zleceniodawca chcę zaoferować dostęp do mocy obliczeniowej jako budżet, żeby nagrodzić pracę bez pieniędzy.
7. Jako wykonawca chcę przyjąć zadanie i śledzić je na tablicy kanban, żeby widzieć postęp.
8. Jako wykonawca chcę dostać punkty za odebrane zadanie, żeby budować rangę.
9. Jako recenzent chcę dostać punkty za recenzje, żeby mój wkład był widoczny.
10. Jako ochotnik chcę udostępnić moc obliczeniową swojego komputera i dostać za to punkty.
11. Jako użytkownik z wysoką rangą chcę prowadzić zespół i moderować, żeby organizować pracę.
12. Jako uczestnik chcę nagrać lub streamować ekran i dołączyć nagranie do zadania, żeby pokazać swoją pracę.
13. Jako użytkownik chcę logować się kontem z narzędzia konferencyjnego, żeby nie zakładać kolejnego konta.
14. Jako użytkownik chcę wyszukiwać projekty i zadania z pomocą AI.
15. Jako uczestnik chcę dostać streszczenie nagrania spotkania.
16. Jako zleceniodawca chcę, żeby AI podpowiedziało pasujących wykonawców.
17. Jako moderator chcę wstępnej moderacji przez AI, ale z decyzją po stronie człowieka.

## Decyzje implementacyjne

### Etap 1 (MVP)
- **Konta, wideokonferencje, nagrywanie i streaming ekranu**: integracja z istniejącym narzędziem Tomasza (ADR 0004).
- **Kod i dane**: P2P w IPFS od początku (ADR 0001). Prywatne pliki są szyfrowane. W IPFS nie ma danych osobowych, bo IPFS nie usuwa danych na żądanie (RODO).
- **Centralnie**: tylko system zadań w stylu Jira z tablicą kanban.
- **Zlecenia**: dowolny podmiot zgłasza zadanie. Budżet jest abstrakcyjny: pieniądze zablokowane w smart kontrakcie (escrow, np. Ethereum) albo dostęp do mocy obliczeniowej. Oba warianty są w etapie 1.
- **Nagrody**: punkty i ranking bez wartości pieniężnej, nieprzekazywalne, więc poza MiCA (motyw 17) (ADR 0002). Ranga rośnie za odebrane zadania, recenzje i udostępnioną moc. Od pewnej rangi można prowadzić zespoły i moderować.
- **AI**: pula Bielika (licencja Apache 2.0; 11B Q4 ok. 12 GB VRAM) lub innego LLM na węzłach ochotników. Każdy węzeł ma cały model, a koordynator rozsyła zapytania (jak AI Horde) (ADR 0003). Zastosowania: wyszukiwanie, streszczenia nagrań, dopasowanie ludzi do zadań, wstępna moderacja. AI jest asystentem, nie decydentem.
- **Ryzyka puli**: węzeł widzi treść zapytania (prywatność), a nieuczciwe węzły mogą oszukiwać. Środki zaradcze: zaufane węzły wysokiej rangi dla danych wrażliwych i wyrywkowa weryfikacja odpowiedzi.

### Etap 2
- Weryfikacja tożsamości autorów przez pośrednika (KIR mojeID, mObywatel u integratora).
- Później logowanie przez Węzeł Krajowy (login.gov.pl). Wymaga wniosku do Ministra Cyfryzacji z wykazaniem interesu faktycznego, polityki bezpieczeństwa i testów integracji.
- Dzielenie modelu między maszyny (Petals, exo, llama.cpp RPC).

## Decyzje testowe

Do ustalenia po wyborze technologii. Testy mają sprawdzać zachowanie widoczne z zewnątrz, a nie szczegóły implementacji.

## Poza zakresem

- Tokeny wymienne na pieniądze.
- Projekty wojskowe lub tajne.
- Współpraca z państwem na starcie.
- Własny system kont i wideokonferencji (jest w narzędziu Tomasza).

## Ryzyka prawne

- Escrow w kryptowalutach/stablecoinach a MiCA: brak polskiego organu wydającego zezwolenia po wecie ustawy, okres przejściowy minimalny do 1.07.2026. Przed startem potrzebna jest konsultacja prawna. Moc obliczeniowa jako budżet nie ma tego ryzyka.
- Lekcja Bountysource: serwis upadł z pieniędzmi użytkowników, dlatego serwis nie przechowuje środków, a wypłata następuje dopiero po odbiorze zadania.

## Kontekst

- Brak polskiego odpowiednika openCode.de/Codeberg.
- SPOIWO i KIGEIT postulują utworzenie repozytorium.
- Polska dołącza do Digital Commons EDIC od 1.01.2027.

## Otwarte pytania

1. Walidacja wykonania zadań: po stronie serwisu czy grona recenzentów?
2. Wybór blockchaina dla escrow.
3. Model moderacji.
4. Nazwa narzędzia Tomasza do konferencji.
