---
description: Запустить касдев-прогрев — telegram-warmer + instagram-warmer в режиме castdev.
---

Запусти двух агентов через Task tool в режиме касдева:

1. `telegram-warmer` с параметром `mode: castdev`
2. `instagram-warmer` с параметром `mode: castdev`

Последовательно, не параллельно (система работает «одна задача за раз»).

**Precheck перед запуском:**
- `research/castdev-brief.md` должен быть готов и в разделе «Выбранный бонус» не должно быть `[TBD]`
- `state.md → castdev_form_url` должен содержать URL формы (не пустое поле)

Если что-то из этого не заполнено — остановись и доложи продюсеру, чего не хватает.

После работы обоих warmer'ов — доложи продюсеру, обнови state.md через orchestrator.
