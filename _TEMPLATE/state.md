# State — [blogger_name]

**Updated:** YYYY-MM-DD HH:MM

> Единственный файл, который читает `orchestrator`. Всегда обновляй после каждого шага агента.

## Статусы

- `pending` — не начато
- `in-progress` — агент работает
- `my-review` — ждёт ревью продюсера
- `client-review` — ждёт ревью эксперта
- `done` — завершено
- `blocked` — заблокировано, нужен ввод
- `skipped` — пропущено (не применимо к этому проекту)

## Config

- `niche_type`: [hard | soft]
- `has_base`: [yes | no]
- `launch_model`: [cold | warm | hybrid | TBD]
- `traffic_channel`: [TG-seed | TG-Ads | Reels | YouTube | N/A | TBD]
- `castdev_form_url`: [URL Google/Яндекс-формы кастдева; продюсер вписывает после создания формы; warmer'ы подставляют автоматически вместо плейсхолдера [ССЫЛКА НА ФОРМУ]]

## Phases

### 1. Onboarding
- **status:** pending
- **owner:** intake-analyst
- **inputs:** `brief/` (все файлы)
- **outputs:** `research/project-digest.md`, `data/blogger-profile.md` (draft)
- **review:** my-review

### 2. Discovery
- **status:** pending
- **owner:** audience-analyst (+ telegram-warmer + instagram-warmer в режиме `castdev` — только при has_base=yes)
- **inputs:** `research/project-digest.md`; при has_base=yes — новые касдев-транскрипты в `brief/public-signals/castdev-responses.md`
- **outputs (has_base=yes):** Проход 1 → `research/audience-hypotheses.md`, `research/money-segments.md`, `research/castdev-questionnaire.md`, `research/castdev-bonus-options.md`, `research/castdev-brief.md`. Проход 2 → `data/audience-profile.md`.
- **outputs (has_base=no):** Проход 1+2 в одном запуске → `research/audience-hypotheses.md`, `research/money-segments.md`, `data/audience-profile.md` (без касдев-артефактов, с пометкой «без верификации»).
- **review:** my-review на research/, client-review на `data/audience-profile.md`

### 3. Strategy
- **status:** pending
- **owner:** audience-analyst → product-architect → launch-strategist
- **outputs:**
  - `data/audience-profile.md` — **client-review** обязателен
  - `data/product-matrix.md` — **client-review** обязателен
  - `data/launch-strategy.md` — **client-review** обязателен
  - `data/warmup-plan.md` — ТЗ для warmers (создаёт launch-strategist)
  - `data/traffic-plan.md` — **только для cold/hybrid** (создаёт launch-strategist)
  - `data/leadmagnet-funnel.md` — **только для cold/hybrid** (создаёт launch-strategist)
  - `research/event-brief.md` — ТЗ для event-writer (Проход 2 стратега, после выбора формата)
- **exit criteria:** `launch_model` и `traffic_channel` в config определены

### 4. Preparation
- **status:** pending
- **branch:** [cold | warm | hybrid] — зависит от `launch_model`
- **owner (cold):** launch-strategist (воронка) + event-writer (если вебинар/ВСЛ) + sales-page-writer + reels-writer (leadmagnet-batch)
- **owner (warm):** telegram-warmer + instagram-warmer + event-writer + sales-page-writer
- **owner (hybrid):** обе ветки параллельно
- **outputs:**
  - `content/tg/` — посты TG (индекс `_index.md` + файлы постов)
  - `content/ig/stories/`, `content/ig/feed/`, `content/ig/reels/` — сторис / лента / рилсы
  - `content/webinar/<NN>-<slug>/` — structure, script, slides, offer-pitch
  - `content/landings/` — registration / offer / leadmagnet / thank-you
- **review:** my-review на прогревы; client-review на сценарий события и offer-лендинг

### 5. Execution
- **status:** pending
- **owner:** продюсер (ведёт вручную, orchestrator следит за критерием закрытия)
- **inputs:** готовый контент в `content/`, стратегия в `data/launch-strategy.md`
- **outputs:** `data/execution-log.md` — лог публикаций, ежедневных отчётов, финальный результат
- **exit criteria:** `data/execution-log.md` → блок «Финал» заполнен (выручка, продажи, уроки) + `status: finalized` в файле → orchestrator ставит фазу `done`

## Журнал событий

<!-- Короткие строки, последние сверху. Формат: YYYY-MM-DD HH:MM | фаза | агент | что сделано -->
