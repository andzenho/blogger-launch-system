---
description: Запустить launch-strategist — стратегия запуска, таймлайн, юнит-экономика, форматы события.
---

Вызови агента `launch-strategist` через Task tool.

**Какой проход автоматически:**
- Нет `data/launch-strategy.md` → Проход 1 (стратегия + warmup-plan + 2-3 варианта формата события + traffic-plan/leadmagnet-funnel для cold/hybrid)
- Есть `data/launch-strategy.md` с зафиксированным форматом → Проход 2 (ТЗ для event-writer → `research/event-brief.md`)

**Precheck:**
- `data/audience-profile.md` готов
- `data/product-matrix.md` готов (client-review done)
- `state.md → config.launch_model` установлен (не `TBD`)

Если `launch_model = TBD` — агент вернёт blocked. Уточни у продюсера: cold / warm / hybrid?

После Прохода 1 → эксперт выбирает формат события → Проход 2.
