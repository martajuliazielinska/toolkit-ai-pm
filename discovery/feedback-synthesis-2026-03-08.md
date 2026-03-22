---
date: 2026-03-08
product: Family Wallet
period: January–February 2026
analyst: Claude (AI PM Assistant)
source: discovery/user-feedback-sample.md
framework: skill:feedback-synthesis
linked_to:
  - analytics/metrics-analysis-2026-03-08.md
  - strategy/strategy-2026-03-08.md
  - logs/decision-2026-03-08.md
---

# 🗣️ Feedback Synthesis — Family Wallet (March 2026)

---

## 1️⃣ FEEDBACK SOURCES

| Źródło | Liczba głosów | Okres | Segment |
|--------|--------------|-------|---------|
| App Store iOS (negatywne) | 31 | Feb 2026 (po v2.1) | Mixed — rodzice 29–52 lat |
| App Store iOS (pozytywne) | 16 | Jan–Feb 2026 | Pocket Money power users |
| App Store Android | 12 | Jan–Feb 2026 | Stabilniejsza baza |
| User interviews — churned | 8 | 24 Feb – 5 Mar | Ex-users, różne powody |
| User interviews — retained | 5 | 1–6 Mar | Pocket Money heavy users |
| Support tickets | 134 | Feb 2026 | 89 crash, 28 CZ, 12 UI, 5 płatności |
| In-app survey (churn risk) | 203 | 15–28 Feb | Trigger: 3 dni bez sesji |

**Łączna próba: ~409 punktów feedbacku**

**Bias i ograniczenia:**
- Nadreprezentacja niezadowolonych (recency bias po v2.1 crash)
- Survey trigger (3 dni bez sesji) wyklucza power users z próby — NPS może być zaniżony
- Czech feedback przez tickets — brak bezpośrednich wywiadów z czeskimi użytkownikami
- Retained users (5 wywiadów) to zbyt mała próba dla wniosków ilościowych

---

## 2️⃣ THEMATIC GROUPING

### Temat A: Stabilność techniczna iOS po v2.1
- **Liczba wzmianek:** 52% survey + 89 tickets + 5 App Store reviews = ~150 punktów
- **Częstotliwość:** HIGH
- Aplikacja crashuje przy logowaniu, ekranie płatności, ładowaniu Pocket Money
- Crash koreluje dokładnie z release iOS v2.1 (15 Feb)
- Android: crash rate 0.4% — problem izolowany do iOS

### Temat B: Nowa nawigacja — "confusing UI"
- **Liczba wzmianek:** 23% survey + 12 tickets + 4 App Store reviews = ~60 punktów
- **Częstotliwość:** HIGH
- Zniknięcie skrótów do Pocket Money z ekranu głównego
- Ikonki bez etykiet niezrozumiałe dla 40+ użytkowników
- Nadmierne ukrycie historii transakcji

### Temat C: Pocket Money — silny PMF signal
- **Liczba wzmianek:** 67% survey + 4 pozytywne reviews + 5 retained interviews = ~165 punktów
- **Częstotliwość:** HIGH (pozytywny)
- Kieszonkowe traktowane jako "killer feature" niezastępowalny przez inne aplikacje
- Dzieci angażują się samodzielnie — zmienia dynamikę rodzinną
- Pain: load time 2.8s odbiera doświadczenie

### Temat D: Czech localization — brak banków
- **Liczba wzmianek:** 28 tickets + 2 user interviews = ~30 punktów
- **Częstotliwość:** MEDIUM (ale CRITICAL dla CZ rynku)
- 100% czeskich tickets dotyczy braku integracji bankowych (Česká spořitelna, Komerční, Moneta)
- Pocket Money chwalony w 11/28 czeskich tickets — produkt rezonuje, infrastruktura nie
- Nest Family wymieniony tylko 6/203 razy — nie jest głównym driver churn w CZ

### Temat E: Support SLA — Premium vs Free
- **Liczba wzmianek:** 2 user interviews + 1 survey cytat + Ticket pattern = ~15 punktów
- **Częstotliwość:** LOW (ale HIGH severity dla Premium segment)
- Premium users oczekują 24h odpowiedzi — dostają >5 dni
- Brak rozróżnienia SLA między Free i Premium = ryzyko utraty najwyżej wartości segmentu

---

## 3️⃣ TOP PAIN POINTS

### Pain Point #1 — iOS v2.1 Crash (CRITICAL)
- **Częstotliwość:** 52% survey, 89/134 tickets, korelat signup -29%
- **Severity:** CRITICAL
- **Segment:** Wszyscy użytkownicy iOS (dominujący rynek)
- **Business impact:** Bezpośrednia przyczyna -29% signups i -40% conversion (crash na ekranie płatności); potencjalne podwójne obciążenia (Ticket #3001)
- **Opis:** Crash przy logowaniu, Pocket Money load, ekranie płatności. Izolowany do iOS 18.x. Android nieaffected.

### Pain Point #2 — "Confusing UI" po redesignie nawigacji (HIGH)
- **Częstotliwość:** 23% survey, 12 tickets, 4 reviews
- **Severity:** HIGH
- **Segment:** Użytkownicy 40+, Pocket Money parents, power users
- **Business impact:** Session duration -43%; churned users nie wracają bo "nie mogę znaleźć"
- **Opis:** Zniknięcie skrótów Pocket Money, ikonki bez etykiet, historia transakcji ukryta. Osoby 40+ szczególnie zagubione.

### Pain Point #3 — Czech localization: brak integracji bankowych (HIGH)
- **Częstotliwość:** 28 tickets (100% CZ), 2 wywiady
- **Severity:** HIGH (bloker dla całego rynku CZ)
- **Segment:** Użytkownicy z Czech Republic (12.1% churn — najgorszy rynek)
- **Business impact:** CZ churn 12.1% vs 8.2% avg; blokuje ekspansję do całego CEE
- **Opis:** Brak integracji z Česká spořitelna, Komerční banka, Moneta. Pocket Money działa — ale bez banku aplikacja jest niekompletna.

### Pain Point #4 — Pocket Money load time 2.8s (MEDIUM → HIGH)
- **Częstotliwość:** 5 retained interviews, survey "71% płaciłoby za Pocket Money z gamifikacją"
- **Severity:** MEDIUM–HIGH (blokuje konwersję w najlepiej rokującym segmencie)
- **Segment:** Pocket Money users — najlojalniejsi i najchętniej płacący
- **Business impact:** Angażują się ale nie konwertują (Paradoks Pocket Money z metrics); load time 87% powyżej celu (<1.5s)
- **Opis:** Dzieci nie mają cierpliwości na 3s load. Rodzice frustrują się za dzieci. Feature love ≠ conversion bez performance.

### Pain Point #5 — Support SLA dla Premium (LOW–MEDIUM)
- **Częstotliwość:** ~15 punktów, 2 wywiady
- **Severity:** MEDIUM (niska skala, ale HIGH impact na Premium churn)
- **Segment:** Premium subscribers (945 — generują 100% MRR)
- **Business impact:** Michał (int. #2) zrezygnował z Premium przez brak odpowiedzi, nie przez crash
- **Opis:** 5 dni na odpowiedź dla Premium usera = utrata zaufania + churn pomimo woli pozostania.

---

## 4️⃣ USER QUOTES (Verbatim)

### Na temat iOS crash:
> "Próbowałam 4 razy. Oddajcie mi moje 10 minut."
— Katarzyna L., ★☆☆☆☆, App Store, 23 Feb

> "Napisałem do supportu. Dostałem automatyczną odpowiedź. Dla mnie to był koniec — nie dlatego że crash, ale dlatego że nikt nie odpowiedział."
— Michał, 41, interview #2, Premium churned

### Na temat UI/nawigacji:
> "Spędziłam 20 minut szukając rzeczy które były na wyciągnięcie ręki."
— Marta W., ★★☆☆☆, App Store, 18 Feb

> "Ikonki bez etykiet — zgaduję co co oznacza. Mam 45 lat i nie jestem designerem."
— Piotr M., ★★☆☆☆, App Store, 20 Feb

### Na temat Pocket Money (pozytywne):
> "Mój syn (11 lat) jest teraz dosłownie dumny ze swojego 'konta'. Żadna rozmowa przy kolacji o pieniądzach nie działała tak dobrze jak ta aplikacja."
— Beata N., ★★★★★, App Store, 12 Jan

> "Córka (9 lat) pyta mnie o kieszonkowe i SAMA sprawdza w aplikacji. To niesamowite."
— Marek Z., ★★★★☆, App Store, 15 Jan

> "Aplikacja zrobiła coś co żaden bank mi nie oferował."
— Renata, 52, interview #7, retained power user

### Na temat Pocket Money (load time):
> "Pocket Money ładuje się bite 3 sekundy. Dla dziecka to wieczność — one nie mają cierpliwości. Proszę to naprawić zanim dzieci stracą zainteresowanie."
— Renata, 52, interview #7, retained power user

### Na temat Czech localization:
> "Pocket Money funguje skvěle, ale bez banky to není kompletní aplikace."
*(tłum: "Pocket Money działa świetnie, ale bez banku to nie jest kompletna aplikacja.")*
— Lucie K., Ostrava, Ticket #3056

> "Kiedy będzie Komerční? Bo jeśli tak — wracam i polecam w biurze."
— Jakub, 38, interview #6, churned Premium (CEE frequent traveller)

### Na temat Support SLA:
> "Lepsza obsługa klienta. 5 dni na odpowiedź to nie premium service."
— respondent #178, In-app survey

> "Nic — aplikacja jest idealna gdy działa. Tylko niech działa."
— respondent #201, In-app survey

### Na temat B2C viral risk:
> "Przez waszą techniczną katastrofę straciliście pięcioosobową rodzinę."
— Paweł, 45, interview #4, churned (rodzina 5 osób)

---

## 5️⃣ PRODUCT INSIGHTS

### Insight 1: Pocket Money to rdzeń PMF — nie aggregator bankowy
- **Obserwacja:** 67% survey, 71% gotowych płacić za gamifikację, 5/5 retained interviews — Pocket Money dominuje pozytywny feedback
- **Interpretacja:** Family Wallet jest postrzegany jako "aplikacja do kieszonkowego dla dzieci z bank aggregatorem", nie odwrotnie. Positioning should follow PMF.
- **Pewność:** HIGH
- **Implikacja:** Pocket Money powinien być primary onboarding flow i hero feature w marketingu, nie feature #3 po budżecie i historii

### Insight 2: iOS v2.1 to egzystencjalne zagrożenie — nie "normalna sytuacja techniczna"
- **Obserwacja:** Crash blokuje acquisition (signups -29%), conversion (płatności crashują), i retencję (Pocket Money load). 3 różne funnel stages affected.
- **Interpretacja:** To nie jest bug — to kryzys reputacyjny. Każdy dzień bez fixa = daily compound damage na wszystkich KPI jednocześnie.
- **Pewność:** HIGH
- **Implikacja:** Crash fix = P0, gate dla WSZYSTKICH innych działań Q2. Marketing restart tylko po crash rate <1%.

### Insight 3: Churn w Czech Republic to infrastruktura, nie konkurencja
- **Obserwacja:** Nest Family wymieniony 6/203 razy. Czech tickets 100% o bankach. Pocket Money chwalony nawet przez churnujących użytkowników CZ.
- **Interpretacja:** Strategia "przyspieszyć ekspansję CZ żeby wyprzedzić Nest Family" jest błędna — problemem nie jest Nest Family, ale brak lokalnych integracji bankowych. Ekspansja bez banków = powtórzenie problemu na nowych rynkach.
- **Pewność:** HIGH
- **Implikacja:** ⚠️ REWIZJA STRATEGII: Czech launch wymaga lokalnych bank integrations PRZED launch, nie po. Zmienia timeline Q3.

### Insight 4: Segment "małe dzieci" (0–7 lat) nie jest gotowy na Pocket Money
- **Obserwacja:** Aleksandra (int. #5, córka 4 lata): "za mała na kieszonkowe — może wrócę za 2-3 lata"
- **Interpretacja:** Onboarding nie segmentuje po wieku dzieci. Rodzice z małymi dziećmi churną bo feature nie jest releventny — a aplikacja ich nie kieruje do wartości którą mogą realnie dostać.
- **Pewność:** MEDIUM (jedna osoba, wymaga weryfikacji)
- **Implikacja:** Onboarding powinien pytać o wiek dzieci i dostosowywać value proposition. "Pocket Money od 8 lat" + "Budżet rodzinny dla każdego"

### Insight 5: B2C viral multiplier — jeden churned user to strata całej rodziny
- **Obserwacja:** Paweł (int. #4): jeden churn = 2 dorośli + 3 dzieci = 5 kont. Rodzina jako unit adoption.
- **Interpretacja:** Tradycyjne modele churn (1 user = 1 churn) zaniżają koszt utraty użytkownika. Prawdziwy LTV i true CAC muszą uwzględniać family multiplier.
- **Pewność:** MEDIUM
- **Implikacja:** Churn prevention dla premium family accounts powinien być priorytetem wyższym niż acquisition — retencja jednej rodziny = retencja 3-5 kont.

---

## 6️⃣ RECOMMENDATIONS

### 🔴 IMMEDIATE (0–1 tydzień)

| Akcja | Owner | Expected Impact |
|-------|-------|----------------|
| **Hotfix iOS v2.1** — crash przy logowaniu, płatnościach, Pocket Money load | Engineering Lead | Odblokowanie acquisition i conversion |
| **Sprawdzić Ticket #3001** — ryzyko podwójnego obciążenia kart | Finance + Engineering | Uniknięcie regulatory/chargeback risk |
| **Support SLA dla Premium** — dedykowana kolejka, 24h response SLA | Customer Success | Zatrzymanie Michałów (Premium churned przez support, nie przez crash) |

### 🟡 SHORT-TERM (1–4 tygodnie)

| Akcja | Owner | Success Metric |
|-------|-------|---------------|
| **UI rollback lub etykiety do ikon** — przywrócenie shortcuts Pocket Money na ekran główny | Design + Engineering | Session duration >6m (z obecnych 4m32s) |
| **50 exit interviews Czech users** — zrozumieć pełny obraz CZ churn | Product Manager | Mapa pain points CZ gotowa do końca marca |
| **Pocket Money performance sprint** — load time <1.5s | Engineering | Adoption 35%+, conversion test możliwy |
| **Onboarding: segmentacja wiekowa dzieci** — pytanie o wiek dziecka w flow | Product + Design | Redukcja churn w segmencie "małe dzieci" |

### 🟢 LONG-TERM (1–3 miesiące)

| Akcja | Roadmap | Uwagi |
|-------|---------|-------|
| **Czech bank integrations** (Česká spořitelna, Komerční, Moneta) | Q3 — PRZED Czech launch | ⚠️ Gate dla ekspansji CZ — bez tego launch nie ma sensu |
| **Pocket Money gamification** (streaks, badges, cele oszczędnościowe) | Q2 end / Q3 | 71% users gotowych płacić — bezpośredni conversion driver |
| **B2B: school partnerships pilot** | Q3 | Pocket Money jako educational tool — naturalny fit |
| **Premium tier: SLA + premium features** | Q2 | Pocket Money Pro + 24h support jako Premium differentiator |

### ❌ DO NOT DO (na bazie feedbacku)

- **Nie restartować marketingu przed crash fixem** — płacenie za złe doświadczenie = niszczenie brand i przepalanie budżetu
- **Nie launchować Czech Republic bez lokalnych banków** — Pocket Money działa, ale aplikacja bez CZ bank = guaranteed churn repeat
- **Nie utrzymywać obecnej nawigacji bez etykiet** — 40+ użytkownicy to core paying segment, alienowanie ich = trwała utrata MRR
- **Nie ignorować Ticket #3001** — potencjalne double charges to ryzyko prawne i reputacyjne

---

## 7️⃣ CORRELATION WITH METRICS

| Pain Point | Powiązana metryka | Korelacja | Hipoteza potwierdzona? |
|-----------|-------------------|-----------|----------------------|
| iOS crash (Pain #1) | New Signups -29%, Conversion -40%, Crash rate 2.1% | STRONG | ✅ TAK — crash od 15 Feb = dokładny dzień iOS v2.1 |
| Confusing UI (Pain #2) | Session duration -43%, D7 Retention 42% | STRONG | ✅ TAK — users nie kończą onboardingu |
| Czech banks (Pain #3) | CZ churn 12.1% vs avg 8.2% | STRONG | ✅ TAK — ale rewizja: Nest Family ≠ główny driver |
| Pocket Money load (Pain #4) | Pocket Money Adoption 28% vs 60% target; Conversion 2.1% | MEDIUM | ✅ TAK — "Pocket Money Paradox" wyjaśniony |
| Support SLA (Pain #5) | Churn 8.2% (component unknown) | WEAK | ⚠️ CZĘŚCIOWO — małe n, ale High-value segment |

**Rewizja Hipotezy C z metrics-analysis:**
> Hipoteza C zakładała: "Nest Family launch (Feb 20) jest głównym driverem CZ churn"
> Feedback mówi: Nest Family wymieniony tylko 6/203 razy. CZ tickets 100% o bankach.
> **Wniosek:** Hipoteza C FALSE. CZ churn = infrastructure problem, nie competitive.

---

## 8️⃣ VALIDATION (Gherkin Format)

### Scenario 1: iOS Crash Fix → Acquisition Recovery
```gherkin
Given crash rate wynosi 2.1% i signups spadły o 29% od iOS v2.1 release (15 Feb)
When Engineering wypuszcza hotfix eliminujący crashi na logowaniu, płatnościach i Pocket Money
Then crash rate spada poniżej 1% w ciągu 48h i dzienny signup rate wraca powyżej 150/dzień w ciągu 2 tygodni
```

### Scenario 2: UI Rollback → Session Duration Recovery
```gherkin
Given średnia sesja wynosi 4m32s (vs cel 8m+) i 23% survey wskazuje na "confusing UI"
When Design przywraca etykiety do ikon i shortcut do Pocket Money na ekranie głównym
Then średnia sesja wzrasta do 6m+ w ciągu 3 tygodni od deploy
```

### Scenario 3: Pocket Money Performance → Conversion Bridge
```gherkin
Given load time Pocket Money wynosi 2.8s i users angażują się (adopcja 28%) ale nie konwertują (2.1%)
When Engineering optymalizuje load time do poniżej 1.5s
Then Pocket Money users konwertują na paid 2× częściej niż non-Pocket Money users (weryfikacja w 4 tygodnie od deploy)
```

---

## ✅ Definition of Ready

- [x] Źródła feedbacku zidentyfikowane i udokumentowane (409 punktów, 4 źródła)
- [x] Thematic grouping zrobiony (5 kategorii)
- [x] Top 5 pain points z severity i business impact
- [x] Cytaty użytkowników verbatim (16 cytatów z różnych źródeł)
- [x] 5 insights produktowych z oceną pewności
- [x] Rekomendacje IMMEDIATE / SHORT / LONG term + DO NOT DO
- [x] Feedback powiązany z 5 metrykami ilościowymi
- [x] Rewizja Hipotezy C (Nest Family ≠ CZ churn driver)
- [x] 3 validation scenarios w Gherkin
- [x] ⚠️ CRITICAL FLAG: Ticket #3001 (double charge risk) + Czech launch gate

---

## 🔗 Powiązane

- Surowy feedback: `/discovery/user-feedback-sample.md`
- Metryki: `/analytics/metrics-analysis-2026-03-08.md`
- Strategia: `/strategy/strategy-2026-03-08.md`
- Decyzje: `/logs/decision-2026-03-08.md`
- Następny krok: `/create-user-story` dla Pocket Money Performance Sprint

*Wygenerowano: 2026-03-08 | Framework: skill:feedback-synthesis*
