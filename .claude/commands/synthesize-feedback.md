---
name: synthesize-feedback
description: Synteza feedbacku użytkowników - grupowanie, wzorce, insights, rekomendacje produktowe
---

# Synteza Feedbacku Użytkowników

## 📋 Co robisz?
Syntetyuzjesz feedback użytkowników z różnych źródeł (App Store, wywiady, support, ankiety) w strukturyzowane insights i rekomendacje produktowe.

## 📂 Gdzie szukasz danych?
1. `/discovery/user-feedback/` - surowy feedback od użytkowników
2. `/discovery/user-research/` - wywiady, badania, ankiety
3. `/analytics/` - metryki zachowań (retencja, churn, sesje)
4. `/strategy/` - kontekst strategiczny i personas

## 🧠 Jak robisz?
Użyj: **skill:feedback-synthesis**

## 📊 Output powinien zawierać:
1. **Źródła feedbacku** - skąd pochodzi, ile głosów, okres
2. **Grupowanie tematyczne** - kategorie problemów i potrzeb
3. **Top Pain Points** - 3-5 najczęstszych problemów z priorytetem
4. **Cytaty użytkowników** - verbatim quotes ilustrujące wzorce
5. **Insights produktowe** - co feedback mówi o produkcie
6. **Rekomendacje** - konkretne akcje dla PM/zespołu
7. **Powiązanie z metrykami** - czy feedback tłumaczy anomalie w danych

## 💾 Gdzie zapisujesz wynik?
→ `/discovery/feedback-synthesis-[data].md`
→ Jeśli impact strategiczny → `/logs/decision-[data].md`

## 🔗 Powiązane:
- skill:feedback-synthesis
- /analyze-metrics
- /create-user-story
- /discovery/user-feedback/
