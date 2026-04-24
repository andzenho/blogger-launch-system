# Карта мультиагентной системы

Справочный файл. **Не грузится в контекст автоматически** — агенты его не читают, это материал для человека: онбординг в систему, объяснение клиенту, напоминание себе «кто что делает».

Если изменилась архитектура (новый агент / новая фаза / перерисовался поток артефактов) — обнови здесь вручную. Это не source of truth, это зеркало `.claude/agents/` + `_TEMPLATE/state.md`.

---

## Слой 1 — Фазы и гейты

Верхнеуровневая последовательность. Каждая фаза имеет владельца-агента и выход, после которого продюсер принимает решение о переходе.

```mermaid
flowchart LR
    O["Onboarding<br/><i>intake-analyst</i>"] --> D["Discovery<br/><i>audience-analyst<br/>+ telegram-warmer</i>"]
    D --> S["Strategy<br/><i>product-architect<br/>launch-strategist</i>"]
    S --> P["Preparation<br/><i>warmers +<br/>event-writer +<br/>sales-page-writer</i>"]
    P --> E["Execution<br/><i>публикация</i>"]

    style O fill:#e3f2fd
    style D fill:#fff3e0
    style S fill:#f3e5f5
    style P fill:#e8f5e9
    style E fill:#eceff1
```

**Правила переходов:**
- Фазы идут строго последовательно. Параллелизма между фазами нет.
- Внутри Preparation warmers и writers работают последовательно (по правилу orchestrator'а — «одна задача за раз»), даже если логически могли бы параллельно.
- Переход между фазами — только по команде продюсера. Автоматических переходов нет.

---

## Слой 2 — Поток агентов и артефактов

Детальная карта: кто что создаёт, что уходит на client-review, где ветвления по модели запуска.

```mermaid
flowchart TB
    BRIEF[(brief/)] --> IA[intake-analyst]
    IA --> DIGEST[research/project-digest.md]
    DIGEST -. пробелы .-> RS[research-scout]
    RS --> BRIEF

    DIGEST --> AA1[audience-analyst<br/>проход 1]
    AA1 --> HYP[research/audience-hypotheses.md<br/>money-segments.md<br/>castdev-questionnaire.md<br/>castdev-bonus-options.md<br/>castdev-brief.md]
    HYP -. has_base=yes .-> TWC[telegram-warmer + instagram-warmer<br/>режим castdev]
    TWC --> CD[(ответы из формы)]
    CD --> AA2[audience-analyst<br/>проход 2]
    AA2 --> AP[data/audience-profile.md<br/>★ client-review]
    AA1 -. has_base=no .-> AP

    AP --> PA[product-architect]
    PA --> PM[data/product-matrix.md<br/>★ client-review]

    PM --> LS1[launch-strategist<br/>проход 1]
    LS1 --> LSTR[data/launch-strategy.md<br/>★ client-review]
    LS1 --> WP[data/warmup-plan.md]
    LS1 -. cold/hybrid .-> TP[data/traffic-plan.md<br/>+ leadmagnet-funnel.md]

    LSTR --> LS2[launch-strategist<br/>проход 2]
    LS2 --> EB[research/event-brief.md]

    EB --> EW[event-writer]
    WP --> TGW[telegram-warmer]
    WP --> IGW[instagram-warmer]
    WP --> RWW[reels-writer<br/>warmup]
    TP --> RWL[reels-writer<br/>leadmagnet]
    LSTR --> SPW[sales-page-writer]

    EW --> C_W[content/webinar/]
    TGW --> C_TG[content/tg/]
    IGW --> C_IG[content/ig/]
    RWW --> C_IG
    RWL --> C_IG
    SPW --> C_L[content/landings/]

    ORC{{orchestrator}} -. читает .-> STATE[(state.md)]
    ORC -. вызывает .-> IA
    ORC -. вызывает .-> AA1
    ORC -. вызывает .-> PA
    ORC -. вызывает .-> LS1
    ORC -. вызывает .-> EW
    ORC -. вызывает .-> TGW
    ORC -. вызывает .-> IGW
    ORC -. вызывает .-> RWW
    ORC -. вызывает .-> SPW

    style AP stroke:#d32f2f,stroke-width:2px
    style PM stroke:#d32f2f,stroke-width:2px
    style LSTR stroke:#d32f2f,stroke-width:2px
    style ORC fill:#fff9c4
```

**Легенда:**
- Красная обводка — артефакт обязательно идёт на `client-review` (клиент визирует).
- Жёлтый `orchestrator` — не производит контент, только диспетчирует.
- Пунктир — условные связи (срабатывают при blocked / при конкретной модели запуска).
- `(brief/)`, `(state.md)` — источники/состояния, не артефакты.

**Два прохода у агентов:**
- `audience-analyst` — проход 1 (гипотезы + касдев-ТЗ до касдева) → проход 2 (финальный профиль после ответов). **Исключение:** при `has_base = no` касдев пропускается, оба прохода идут в одном запуске — на выходе сразу `audience-profile.md` с пометкой «без верификации».
- `launch-strategist` — проход 1 (стратегия + warmup-plan) → проход 2 (event-brief после выбора формата эксперта).

**Ветвление по `has_base` (фаза Discovery):**
- `yes` — полный цикл: Проход 1 → касдев (TG + IG, mode=castdev) → Проход 2. Касдев-артефакты (`castdev-questionnaire.md`, `castdev-bonus-options.md`, `castdev-brief.md`) создаются Проходом 1.
- `no` — касдев физически невозможен (постить некуда). Проход 1 + 2 за один запуск, касдев-артефакты не создаются.

**Ветвление по `launch_model`:**
- `warm` — база есть, трафик не нужен. Активны только `warmup-plan` + TG/IG warmers.
- `cold` — базы нет. Добавляется `traffic-plan` + `leadmagnet-funnel` + `reels-writer leadmagnet` + холодный лендинг.
- `hybrid` — обе ветки параллельно.

---

## Слой 3 — Skills → Agents

Где живёт методология и какой агент её читает. Skills — это переносимые «мозги»; agents — исполнители, склеивающие skill с контекстом проекта.

```mermaid
flowchart LR
    subgraph skills[".claude/skills/"]
        SW[webinar-writing]
        STG[telegram-warming]
        SIG[instagram-warming]
        SVR[viral-reels]
        SSP[sales-page-writing]
        SLS[launch-strategy]
    end

    subgraph agents[".claude/agents/"]
        EW[event-writer]
        TGW[telegram-warmer]
        IGW[instagram-warmer]
        RW[reels-writer]
        SPW[sales-page-writer]
        LS[launch-strategist]
    end

    SW --> EW
    STG --> TGW
    SIG --> IGW
    SVR --> RW
    SSP --> SPW
    SLS --> LS
```

**Агенты без skill-файла** (методология встроена прямо в промпт агента, отдельного skill нет):
- `intake-analyst`
- `audience-analyst`
- `product-architect`
- `research-scout`
- `orchestrator`

Если методология для одного из них разрастается — это сигнал вынести её в `.claude/skills/<topic>/`. Пример — `launch-strategist` изначально держал методологию в промпте, но после роста (~300 строк юнит-экономики + волн + форматов) её вынесли в skill `launch-strategy`.

---

## Статусы в `state.md`

Словарь, который использует `orchestrator` при обновлении фаз:

| Статус | Смысл |
|---|---|
| `pending` | не начато |
| `in-progress` | агент работает |
| `my-review` | ждёт ревью продюсера |
| `client-review` | ждёт ревью эксперта |
| `done` | завершено |
| `blocked` | заблокировано, нужен ввод |
| `skipped` | пропущено (не применимо к этому проекту) |

---

## Как обновлять эту карту

- Добавили нового агента → нарисовать его в слое 2 + (если есть skill) в слое 3.
- Изменилась структура фаз в `_TEMPLATE/state.md` → синхронизировать слой 1.
- Поменялись артефакты-выходы → обновить слой 2.
- Добавили skill → слой 3.

Если поленились обновить — никто не умрёт, но новый человек будет читать `.claude/agents/*.md` напрямую, и это медленнее.
