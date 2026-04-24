---
description: Запустить intake-analyst — парсинг brief/ и сборка research/project-digest.md.
---

Вызови агента `intake-analyst` через Task tool. Он прочитает всю папку `brief/` текущего проекта и соберёт `research/project-digest.md`.

Используй, когда:
- Клиент принёс заполненный `brief/client-intake.md` и транскрипт `brief/discovery-call.md` → первый запуск в Onboarding
- Клиент обновил любой файл в `brief/` → нужно пересобрать digest

После завершения агента обнови state.md через orchestrator и доложи продюсеру.
