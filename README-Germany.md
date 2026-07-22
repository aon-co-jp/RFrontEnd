# RFrontEnd

Ein übergeordnetes Repository, das mehrere Projekte bündelt, die HTML5-/
CSS3-/TypeScript-/React-Äquivalente vollständig von Grund auf entwickeln,
ohne jeglichen Code aus bestehenden Implementierungen wiederzuverwenden.

📖 Weitere Sprachen: [日本語](README-Japan.md) / [English](README-English.md) /
[中文](README-Chinese.md) / [한국어](README-Korea.md) / [Español](README-Spain.md) /
[Français](README-France.md) / [Deutsch](README-Germany.md) / [Italiano](README-Italy.md) /
[Русский](README-Russia.md) / [العربية](README-Arabic.md) —
Für die Einbindung in ein anderes Projekt siehe **[PORTING.md](PORTING.md)**,
eine einzige, in sich geschlossene Datei.

## Untergeordnete Projekte

| Projekt | Rolle | Repository |
|---|---|---|
| RHTML | HTML5-äquivalenter Parser/DOM-Implementierung | [RTHML](https://github.com/aon-co-jp/RTHML) |
| RCSS | CSS3-äquivalenter Parser/Kaskadenberechnung | [RCSS](https://github.com/aon-co-jp/RCSS) |
| RTypeScript | TypeScript-Äquivalent | [RTypeScript](https://github.com/aon-co-jp/RTypeScript) |
| RJSON | JSON-Verarbeitung | [RJSON](https://github.com/aon-co-jp/RJSON) |
| RReact | React-äquivalentes Komponentenmodell | [RReact](https://github.com/aon-co-jp/RReact) |

Die serverseitige Laufzeitumgebung wird von
[RPoem](https://github.com/aon-co-jp/RPoem) (Poem von Grund auf neu
entwickelt) und [RCosmo](https://github.com/aon-co-jp/RCosmo) (ehemals
open-runo) übernommen.

Ausführliche Entwicklungsrichtlinien, Build-Reihenfolge und Architektur
siehe `CLAUDE.md`.

## Lizenz

Apache-2.0 OR MIT
