# 📊 Toolkit PM - Akademicki Case Study

## 🎯 Cel Projektu
System do zarządzania pracą PM'a z AI. Lustrzane odbicie workflow'u pracy.

## 📁 Struktura Katalogów

### /discovery
Research, competitive analysis, user feedback, market trends
- Gdzie szukamy: konkurencja, użytkownicy, trendy rynkowe

### /strategy
Wizja produktu, SWOT, decyzje strategiczne, roadmap
- Gdzie przechowujemy: strategie, cele, wizja

### /backlog
User Stories, Epiki, requirements, zadania do zrobienia
- Gdzie trzymamy: rzeczy do implementacji

### /execution
Szablony, procesy, how-to guides, standardy
- Gdzie są: instrukcje, formaty, metodyka

### /analytics
Metryki, raporty, analizy, KPI'e
- Gdzie analizujemy: performance, trends

### /logs
Historia decyzji, meeting notes, dlaczego coś podjęliśmy
- Gdzie logujemy: WHY behind decisions

### /.claude
Komendy i skille dla projektu
- /commands - dostępne komendy
- /skills - dostępne umiejętności

## 🔧 Dostępne Komendy
- `/analyze-competitors` - analiza konkurencji
- `/create-user-story` - tworzenie User Story
- `/analyze-metrics` - analiza metryk produktowych
- `/review-metrics` - review metryk
- `/plan-strategy` - planowanie strategii
- `/synthesize-feedback` - synteza feedbacku użytkowników

## 📚 Dostępne Skille
- `skill:user-story-template-jira` - User Story w Jira standardzie
- `skill:metrics-analysis-framework` - Framework do analizy metryk
- `skill:strategy-framework` - Framework do planowania strategii
- `skill:feedback-synthesis` - Framework do syntezy feedbacku

## 📋 Reguły Pracy
1. Wszystkie dane strategiczne → /strategy
2. Wszystkie decyzje → loguj w /logs
3. Każdy User Story → template z skill:user-story-template
4. Competitive research → zawsze w /discovery
5. Przed zmianą struktury - update tego pliku

## Project Principles

- **Role-Based Access:** Parent/Child/Advisor/Admin accounts with distinct permissions
- **No-Show Policy:** Defined cancellation + no-show rules for consultant bookings

## 🚀 Quick Start
1. Czytaj ten plik: CLAUDE.md
2. Wyszukaj komendę: /analyze-competitors
3. Claude będzie wiedzieć gdzie szukać danych
4. Użyj skill'u do strukturalizacji wyniku
