# RFrontEnd

A parent repository that ties together multiple projects, each developing
HTML5/CSS3/TypeScript/React equivalents entirely from scratch, without
reusing any code from existing implementations.

📖 Other languages: [日本語](README-Japan.md) / [English](README-English.md) /
[中文](README-Chinese.md) / [한국어](README-Korea.md) / [Español](README-Spain.md) /
[Français](README-France.md) / [Deutsch](README-Germany.md) / [Italiano](README-Italy.md) /
[Русский](README-Russia.md) / [العربية](README-Arabic.md) —
To integrate this project elsewhere, see **[PORTING.md](PORTING.md)**, a single self-contained file.

## Subordinate projects

| Project | Role | Repository |
|---|---|---|
| RHTML | HTML5-equivalent parser/DOM implementation | [RTHML](https://github.com/aon-co-jp/RTHML) |
| RCSS | CSS3-equivalent parser/cascade computation | [RCSS](https://github.com/aon-co-jp/RCSS) |
| RTypeScript | TypeScript equivalent | [RTypeScript](https://github.com/aon-co-jp/RTypeScript) |
| RJSON | JSON processing | [RJSON](https://github.com/aon-co-jp/RJSON) |
| RReact | React-equivalent component model | [RReact](https://github.com/aon-co-jp/RReact) |

The server-side execution runtime is handled by
[RPoem](https://github.com/aon-co-jp/RPoem) (Poem redeveloped from scratch)
and [RCosmo](https://github.com/aon-co-jp/RCosmo) (formerly open-runo).

See `CLAUDE.md` for detailed development policy, build order, and architecture.

## License

Apache-2.0 OR MIT
