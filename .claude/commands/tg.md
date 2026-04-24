---
description: Запустить telegram-warmer — посты прогрева в Telegram-канале.
---

Вызови агента `telegram-warmer` через Task tool.

**Precheck:**
- `data/warmup-plan.md` готов (launch-strategist Проход 1 завершён)
- `data/audience-profile.md`, `data/product-matrix.md`, `data/launch-strategy.md` на месте

**Режимы** (уточни у продюсера):
- `warming-plan` — индекс постов на весь прогрев (план 4 волн)
- `warming-write` — написать конкретный пост или несколько
- `castdev` — специальный режим: 3-4 дня прогрева под касдев-анкету (требует готовый `research/castdev-brief.md` + `state.md → castdev_form_url`)
- `adhoc` — произвольная задача (правка, переписать, ad-hoc пост)

По умолчанию начинай с `warming-plan` → после утверждения продюсером переходи к `warming-write` по дням.

Артефакты в `content/tg/`.
