# examples/

Реальные примеры постов прогрева с разбором. Агент `telegram-warmer` читает отсюда, чтобы ориентироваться на стиль и структуру.

## Как добавлять пример

Один файл = один пост. Имя: `<format>-<short-slug>.md`, например `expert-3-mifa-o-zapuskakh.md`, `student_case-anna-200k.md`.

Шапка:

```
---
format: expert | life | personality | selling_story | podcast_theses | student_case | own_result | live_promo | challenge | mission
wave: W1 | W2 | W3 | W4
niche: запуски в TG | нутрициология | крипта | …
source: <проект/блогер, если можно указать, иначе «анонимизировано»>
length: <кол-во знаков>
---

## Что работает в этом посте
- <приём 1>
- <приём 2>

## Текст

<сам текст поста>

## Разбор
<1–3 абзаца: почему именно так, на какой сегмент, что скопировать, что НЕ копировать>
```

## Что уже лежит

### Отдельные посты (9 штук)

Собраны из двух реальных прогревов (анонимизированная ВИП-группа по ТГ-запускам + крипто-обучение Crypto Insider):

- **expert (W2)** — [`expert-metod-sibiryaka-myagkie-nishi.md`](expert-metod-sibiryaka-myagkie-nishi.md), [`expert-2-strategii-crypto.md`](expert-2-strategii-crypto.md)
- **student_case (W3)** — [`student_case-dinara-100mln.md`](student_case-dinara-100mln.md), [`student_case-pepe-x15.md`](student_case-pepe-x15.md)
- **personality (W1)** — [`personality-15k-dollarov-poterял.md`](personality-15k-dollarov-poterял.md)
- **selling_story (W4)** — [`selling_story-otkrytie-prodazh.md`](selling_story-otkrytie-prodazh.md)
- **own_result (W1)** — [`own_result-55-procentov-tramp.md`](own_result-55-procentov-tramp.md)
- **live_promo (день эфира)** — [`live_promo-3-chasa-do-efira.md`](live_promo-3-chasa-do-efira.md) (серия из 3 постов: −3ч / −1ч / −15 мин)
- **mission (W2)** — [`mission-bez-kriptanskogo-slenga.md`](mission-bez-kriptanskogo-slenga.md)

Покрыты обе полярности ниш: твёрдая (инфобиз/запуски, высокая требовательность к методу и кейсам) и мягкая (крипта-новички, акцент на снятие страха и упаковку метода без жаргона). Можно добавлять ещё — особенно `challenge`, `podcast_theses` и второй `selling_story` из середины окна продаж.

### Разбор целой кампании (1 штука)

- [`case-study-pyrikov-mentoring-2025.md`](case-study-pyrikov-mentoring-2025.md) — публичный разбор 13-недельного запуска менторства в ТГ (Егор Пыриков, январь–апрель 2025). Multi-channel ecosystem: основной канал + канал закрытой презентации + канал разборов + канал предобучения + сторис + бот. Эталон «большой машины» для крупного эксперта с базой 20k+.

Разборы кампаний читай, когда нужно спроектировать структуру прогрева, а не отдельный пост. Они отвечают на вопросы: сколько каналов нужно, как распределять контент между ними, какие сюжетные линии тянуть месяцами, как раскладывать таймлайн анонсов.
