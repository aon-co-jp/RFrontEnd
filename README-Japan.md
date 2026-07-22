# RFrontEnd

HTML5/CSS3/TypeScript/React相当を、既存実装のコードを一切流用せず一から開発する複数プロジェクトを束ねる親リポジトリ。

📖 詳細: [日本語 README](README-Japan.md) / [English README](README-English.md) /
[中文](README-Chinese.md) / [한국어](README-Korea.md) / [Español](README-Spain.md) /
[Français](README-France.md) / [Deutsch](README-Germany.md) / [Italiano](README-Italy.md) /
[Русский](README-Russia.md) / [العربية](README-Arabic.md) —
他プロジェクトへの導入は **[PORTING.md](PORTING.md)** 1 枚で完結します。

## 傘下プロジェクト

| プロジェクト | 役割 | リポジトリ |
|---|---|---|
| RHTML | HTML5相当のパーサー/DOM実装 | [RTHML](https://github.com/aon-co-jp/RTHML) |
| RCSS | CSS3相当のパーサー/カスケード計算 | [RCSS](https://github.com/aon-co-jp/RCSS) |
| RTypeScript | TypeScript相当 | [RTypeScript](https://github.com/aon-co-jp/RTypeScript) |
| RJSON | JSON処理 | [RJSON](https://github.com/aon-co-jp/RJSON) |
| RReact | React相当のコンポーネントモデル | [RReact](https://github.com/aon-co-jp/RReact) |

サーバー側の実行基盤は[RPoem](https://github.com/aon-co-jp/RPoem)(Poemを一から再開発)、[RCosmo](https://github.com/aon-co-jp/RCosmo)(旧open-runo)が担う。

詳しい開発方針・開発順序・アーキテクチャは`CLAUDE.md`を参照。

## ライセンス

Apache-2.0 OR MIT
