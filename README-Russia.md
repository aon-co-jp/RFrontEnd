# RFrontEnd

Родительский репозиторий, объединяющий несколько проектов, которые
разрабатывают эквиваленты HTML5/CSS3/TypeScript/React полностью с нуля, не
используя повторно код существующих реализаций.

📖 Другие языки: [日本語](README-Japan.md) / [English](README-English.md) /
[中文](README-Chinese.md) / [한국어](README-Korea.md) / [Español](README-Spain.md) /
[Français](README-France.md) / [Deutsch](README-Germany.md) / [Italiano](README-Italy.md) /
[Русский](README-Russia.md) / [العربية](README-Arabic.md) —
Чтобы интегрировать этот проект в другой, см. **[PORTING.md](PORTING.md)** —
единственный самодостаточный файл.

## Дочерние проекты

| Проект | Роль | Репозиторий |
|---|---|---|
| RHTML | Парсер/реализация DOM, эквивалентная HTML5 | [RTHML](https://github.com/aon-co-jp/RTHML) |
| RCSS | Парсер/расчёт каскада, эквивалентный CSS3 | [RCSS](https://github.com/aon-co-jp/RCSS) |
| RTypeScript | Эквивалент TypeScript | [RTypeScript](https://github.com/aon-co-jp/RTypeScript) |
| RJSON | Обработка JSON | [RJSON](https://github.com/aon-co-jp/RJSON) |
| RReact | Модель компонентов, эквивалентная React | [RReact](https://github.com/aon-co-jp/RReact) |

За серверную среду выполнения отвечают
[RPoem](https://github.com/aon-co-jp/RPoem) (Poem, переработанный с нуля) и
[RCosmo](https://github.com/aon-co-jp/RCosmo) (ранее open-runo).

Подробную политику разработки, порядок сборки и архитектуру см. в `CLAUDE.md`.

## Лицензия

Apache-2.0 OR MIT
