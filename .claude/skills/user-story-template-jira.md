---
name: user-story-template-jira
description: Template User Story dla Jira - standard z pracy (Context, Benefits, Scope, NRF, AC)
---

# 📋 User Story Template - Jira Standard

## 1️⃣ CONTEXT
**Cel:** Opisać business problem lub opportunity które próbujemy rozwiązać

> [WPISZ: Jaki problem biznesowy adresujemy? Jaka okazja?]

**Właściciel:** Product Owner (przed Q&S backlog review)

---

## 2️⃣ OBJECTIVES & USER STORY
**Format:**
```
As a [ROLE/USER TYPE]
I want [FEATURE/CAPABILITY]
So that [BUSINESS VALUE/BENEFIT]
```

**Przykład:**
```
As a Business Admin User
I want to easily configure product merchandise fields
So that I can effectively manage product catalog
```

---

## 3️⃣ BENEFITS
**Cel:** Opisać high-level benefity z delivery'u tej zmiany

> [WPISZ: Jakie konkretne benefity realize'emy?]

**Właściciel:** Product Owner (quality & size process)

---

## 4️⃣ KPI & COST-BENEFIT
**Cel:** Zmierzyć ROI zmiany

**KPI (Key Performance Indicators):**
- [WPISZ: Metryka 1]
- [WPISZ: Metryka 2]
- [WPISZ: Jak będziemy to mierzyć?]

**Cost-Benefit:**
- Cost of change: [WPISZ]
- Expected benefit: [WPISZ]
- ROI: [WPISZ]

**Właściciel:** Product Owner (quality & size process)

---

## 5️⃣ SCOPE

### ✅ IN SCOPE (Co robimy)
- **Markets:** [WPISZ: które rynki?]
- **Processes:** [WPISZ: które procesy?]
- **Users:** [WPISZ: którzy użytkownicy?]

### ❌ OUT OF SCOPE (Co NIE robimy)
- [WPISZ: Co jest poza scopem?]
- [WPISZ: Co robimy w Future?]

### 👥 KEY STAKEHOLDERS
- [WPISZ: kto się tym zajmuje?]
- [WPISZ: kto podejmuje decyzje?]

### ⏰ TIMELINE
- **Critical Timeline:** [WPISZ: jakie daty?]
- **Legal Timeline (if applicable):** [WPISZ]
- **Must-have by:** [WPISZ: due date]

---

## 6️⃣ NON-FUNCTIONAL REQUIREMENTS (NFR)

### Usability
> Business Admin User musi łatwo używać konfiguracji i rozumieć purpose każdego field'a aby efektywnie merchandisować produkty.

**Wymogi:**
- [WPISZ: co User musi zrobić bez problemów?]
- [WPISZ: jakie są pain points?]

### Impacted Systems
**TDA Required:** [TAK/NIE - czy wymagana Architecture Review?]

**Systemy do sprawdzenia:**
- [WPISZ: System 1]
- [WPISZ: System 2]

**Architects Input:**
- [WPISZ: co architekci muszą znać?]

### Test Considerations
- [WPISZ: co QA musi testować?]
- [WPISZ: edge cases?]

### Accessibility Requirements
- [WPISZ: WCAG level?]
- [WPISZ: jakie requirements?]

### Dev Notes
- [WPISZ: technical constraints]
- [WPISZ: tech stack considerations]

### QA Notes
- [WPISZ: test scenarios]
- [WPISZ: regression areas]

---

## 7️⃣ ACCEPTANCE CRITERIA (Gherkin Format)

### Scenario 1: [Nazwa scenariusza]
```gherkin
Given [WPISZ: początkowy stan]
When [WPISZ: użytkownik robi coś]
Then [WPISZ: rezultat]
```

### Scenario 2: [Nazwa scenariusza]
```gherkin
Given [WPISZ: początkowy stan]
When [WPISZ: użytkownik robi coś]
Then [WPISZ: rezultat]
```

### Scenario 3: [Nazwa scenariusza]
```gherkin
Given [WPISZ: początkowy stan]
When [WPISZ: użytkownik robi coś]
Then [WPISZ: rezultat]
```

---

## ✅ Definition of Ready (DoR)
- [ ] Context opisany
- [ ] User Story zdefiniowana
- [ ] Benefits zidentyfikowane
- [ ] KPI ustalone
- [ ] Scope jasny (in/out)
- [ ] Stakeholders identified
- [ ] NFR określone
- [ ] AC w Gherkin'ie
- [ ] Product Owner approve

---

## 🔗 Powiązane
- Komenda: `/create-user-story`
- Folder: `/backlog/`
