---
name: analyze-metrics
description: Analiza metryk produktowych - KPI, trendy, rekomendacje w standardzie metrics-framework
---

# Analiza Metryk Produktowych

## 📋 Co robisz?
Analizujesz metryki produktu, identyfikujesz trendy, anomalie i generujesz rekomendacje dla zespołu.

## 📂 Gdzie szukasz danych?
1. `/analytics/` - dane metryczne, raporty, KPI
2. `/strategy/product-vision.md` - cele produktowe
3. `/backlog/` - features w pipeline
4. `/logs/` - poprzednie decyzje oparte na metrykach

## 🧠 Jak robisz?
Użyj: **skill:metrics-analysis-framework**

## 📥 INPUT
Podaj nazwę pliku z danymi metrycznymi (np. @product-metrics-sample.md)
lub wklej dane bezpośrednio do wiadomości.
Claude automatycznie dopasuje input do workflow.

## 📋 Output powinien zawierać:
1. Current State - aktualny stan KPI vs target
2. Trends - co rośnie, co spada, co stoi
3. Anomalies - co odbiega od normy
4. Root Cause Analysis - hipotezy dlaczego
5. Recommendations - konkretne następne kroki

## 💾 Gdzie zapisujesz wynik?
→ `/analytics/metrics-analysis-[data].md`

## 🔗 Powiązane:
- skill:metrics-analysis-framework
- /strategy/product-vision.md
- /logs/
