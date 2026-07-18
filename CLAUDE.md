# 開発方針＆開発環境ルール(RFrontEnd)

作業ドライブは`F:\open-runo`。この節は[`open-raid-z`](https://github.com/aon-co-jp/open-raid-z)の`CLAUDE.md`を正本とし、各プロジェクトへコピーして同期する方針に準じる。

## このリポジトリの役割(2026-07-18新設)

`RFrontEnd`は、HTML5/CSS3/TypeScript/React相当を既存実装のコードを
一切流用せず一から開発する複数プロジェクトを束ねる親リポジトリ
(open-runoドライブ全体に対する`F:\open-runo\CLAUDE.md`と同じ位置づけ)。
各子プロジェクトは個別のGitHubリポジトリ・個別のcrateとして開発され、
このリポジトリ自体はコード本体を持たず、全体構成・開発順序・
プロジェクト間の関係を記録する。

## 傘下プロジェクト

| プロジェクト | 役割 | GitHub | ローカル | VPS |
|---|---|---|---|---|
| RHTML | HTML5相当のブラウザエンジン級パーサー/DOM実装 | [RTHML](https://github.com/aon-co-jp/RTHML) | `F:\open-runo\RFrontEnd\RHTML` | `/root/RFrontEnd/RHTML` |
| RCSS | CSS3相当のパーサー/カスケード/スタイル計算 | [RCSS](https://github.com/aon-co-jp/RCSS) | `F:\open-runo\RFrontEnd\RCSS` | `/root/RFrontEnd/RCSS` |
| RBootStrap | Bootstrap相当のCSSフレームワーク(グリッド+基本コンポーネント) | [RBootStrap](https://github.com/aon-co-jp/RBootStrap) | `F:\open-runo\RFrontEnd\RBootStrap` | `/root/RFrontEnd/RBootStrap` |
| RTypeScript | TypeScript相当(クライアント側ロジック、Wasm化) | [RTypeScript](https://github.com/aon-co-jp/RTypeScript) | `F:\open-runo\RFrontEnd\RTypeScript` | `/root/RFrontEnd/RTypeScript` |
| RJSON | JSON処理(旧`Rust-JSON`を改称・移動) | [RJSON](https://github.com/aon-co-jp/RJSON) | `F:\open-runo\RFrontEnd\RJSON` | `/root/RFrontEnd/RJSON` |
| RReact | React(React DOM/Native/Mobile相当)のコンポーネントモデル | [RReact](https://github.com/aon-co-jp/RReact) | `F:\open-runo\RFrontEnd\RReact` | `/root/RFrontEnd/RReact` |
| RGraphQL | GraphQL仕様の自前実装(トークナイザ/パーサー、v0.2.0でmutation/変数/フラグメント対応) | [RGraphQL](https://github.com/aon-co-jp/RGraphQL) | `F:\open-runo\RFrontEnd\RGraphQL` | `/root/RFrontEnd/RGraphQL` |
| RNode.js | Node.jsコア概念の自前実装(CommonJS解決・イベントループ・実行時ドライバ) | [RNode.js](https://github.com/aon-co-jp/RNode.js) | `F:\open-runo\RFrontEnd\RNode.js` | `/root/RFrontEnd/RNode.js` |

**傘下ではないが関連するプロジェクト**(このリポジトリの外、`RFrontEnd`
ディレクトリの外に配置):
- [RPoem](https://github.com/aon-co-jp/RPoem)(旧`poem-cosmo-tauri`。
  Poemフレームワーク自体を一から再開発したサーバー側実行基盤) —
  `F:\open-runo\RPoem`、VPS `/root/RPoem`
- [RCosmo](https://github.com/aon-co-jp/RCosmo)(旧`open-runo`) —
  `F:\open-runo\RCosmo`、VPS `/root/RCosmo`

## アーキテクチャ方針

- **RHTML/RCSS/RTypeScript/RJSON/RReactは、既存のHTML5/CSS3/
  TypeScript/JSON/Reactの実装コードを一切流用せず、一から開発する**
  (簡易テンプレートエンジンではなくブラウザエンジン級の再実装、
  詳細はRHTML/RCSS各リポジトリのCLAUDE.md参照)。
- **開発順序**: RHTML/RCSS(土台)→ RBootStrap → RTypeScript
  (RReactは別プロジェクトで並行開発)。
- **将来的にSSR(Server-Side Rendering)まで見据える**: RPoem(サーバー側
  実行基盤)上でRHTMLがDOM木を構築 → RCSSでスタイル計算 → HTML文字列
  としてレスポンス。クライアントはブラウザ標準でパース(初期表示)、
  後からRTypeScript(Wasm)がハイドレーション。
- **層の役割の整理**(ユーザー指示、2026-07-18): Rust+tokio/hyper
  (RPoemの基盤)は**サーバー側の実行基盤**、WASMは**ブラウザ側の
  実行環境**——両者は比較対象ではなく役割が異なる。

## お引越し可能な設計判断

- **DOM実装非依存のCSS処理**(RCSS): `RCSS::ElementLike`トレイト
  経由で要素とマッチングする設計は、`RHTML::Element`以外のDOM実装
  (将来のRBootStrap・別のUIツールキット等)にもそのまま応用できる。
- **`TokenSink`パターン**(RHTML): トークナイザとDOM構築器を疎結合に
  する設計は、CSS・将来のRTypeScript構文解析にも応用可能な汎用パターン。
- **`Patch`ベースの差分適用**(RReact): 仮想DOMの差分計算結果を
  `RHTML::Node`への実適用に変換するアダプタは、将来RReact↔RHTMLを
  接続する際にそのまま流用できる設計にしてある。RHTML↔RCSS↔RReactの
  「DOM木構築→スタイル解決→VNode化」までは2026-07-18に接続済み
  (`RReact`の`dom_bridge`フィーチャ)、`Patch`の実DOM反映は未接続。

## 現状(2026-07-18更新)

- RHTML: トークナイザ+DOM木構築器、13テストgreen。
- RCSS: セレクタ/パーサー/カスケード計算+子孫結合子・子結合子(`>`)・
  隣接兄弟結合子(`+`)対応、27テストgreen。
- RJSON: 旧Rust-JSONの内容をそのまま移行(パス依存先の
  `audiocafe-tokyo-server`・`aruaru-db`も更新済み)。
- RReact: 仮想DOM+差分計算(reconciliation)、10テストgreen。加えて
  `dom_bridge`フィーチャ(既定では無効)でRHTML/RCSSと接続する
  最小のEnd-to-Endパイプライン(RHTMLでDOM木構築→RCSSでスタイル解決→
  RReactのVNode木変換)を実装、フィーチャ有効時は16テストgreen
  (RCSSの子/隣接兄弟結合子対応に追従、兄弟ノード追跡を追加)。
- RTypeScript: 最小スコープ(単純な型注釈の削除・JSへの
  トランスパイルのみ)で新規実装、14テストgreen。GitHubリポジトリ
  `aon-co-jp/RTypeScript`へpush済み。
- RBootStrap: グリッドシステム(container/row/col-*)+基本コンポーネント
  (button/card/navbar)のクラス名パーサー、22テストgreen。
  `aon-co-jp/RBootStrap`へpush済み(2026-07-18、独立リポジトリとして
  未初期化・未pushのまま放置されていたのを巡回中に発見・修正)。
- RGraphQL: v0.2.0スコープ(mutation・変数定義・フラグメント・
  ディレクティブ)まで拡張、24テストgreen。依存クレートは引き続き
  ゼロ。`aon-co-jp/RGraphQL`へpush済み。
- RNode.js: CommonJS解決(実I/O版含む)・イベントループ・
  `TokioDriver`(実時間駆動)、36テストgreen。**実バグ発見・修正
  (2026-07-18)**: `TokioDriver::wait_until`が`Handle::block_on`経由で
  永久にハングする不具合があり、`std::thread::sleep`ベースの実装に
  修正(合わせてtokio依存自体を撤去、ゼロ依存を維持)。
  `aon-co-jp/RNode.js`へpush済み。

## 次にすべきこと

1. RTypeScriptの本格実装方針決定(3案: A=独自VM実行、B=Rustネイティブ
   DSL+Wasm直接コンパイル、C=swcでASTのみ取り込み+独自インタプリタ。
   B案から着手しC案へ拡張するのが現実的という暫定方針)。第一段の
   型注釈除去トランスパイラは実装済み。
2. RHTML↔RCSS↔RReactの相互接続の深化(最小パイプラインは実装済み
   ——`RReact`の`dom_bridge`フィーチャ参照。次は`Patch`の実DOM反映
   ・コンポーネントモデル)
3. RPoem上での最小SSRエンドポイント(RHTML+RCSSだけで完全なHTMLを
   返す、というマイルストーン)
4. RBootStrapのflexbox折り返し計算・レスポンシブ`@media`出力
5. RCSSの一般兄弟結合子(`~`)対応・`@media`/`!important`(子結合子・
   隣接兄弟結合子は2026-07-18対応済み)
6. RGraphQLのvalidation/execution層(v0.1.0/v0.2.0はパーサーのみ)
7. RNode.jsのコアモジュール(fs以外の http/path等)実I/O実装

## 契約不要の独自AI・「分身の術」構成について(2026-07-18追記、正本はopen-raid-z参照)

`open-raid-z/CLAUDE.md`に、(1) 外部AI事業者との有償契約を必要としない
`open-cuda` + `aruaru-llm` のSET構成、(2) `open-web-server`の「分身の術」
(共有バックエンドインスタンスへの動的テナント登録、ドメインごとの
個別インストール不要)の対象拡大、という2つのエコシステム方針が
記録されている。

**このリポジトリ(RHTML/RCSS/RTypeScript/RJSON/RReact)への適用について
の判断**: これらはいずれも**ライブラリ/コンパイル時コンポーネント**
であり、単体でHTTPサービスとして稼働するものではないため、「分身の術」
(動的テナント登録)は直接は適用されない。ただし「次にすべきこと」#3の
**「RPoem上での最小SSRエンドポイント」が実装された段階では、そのSSR
エンドポイント自体は`open-web-server`と同じ「分身の術」パターン
(共有RPoemインスタンスへドメインを動的登録、個別インストール不要)に
従うべき**——RPoem側のCLAUDE.mdに記載済みの方針を、RFrontEnd由来の
SSR機能を提供する際にも踏襲すること。

## 関連プロジェクト

- [open-raid-z](https://github.com/aon-co-jp/open-raid-z) — 開発ルールの正本
- [RPoem](https://github.com/aon-co-jp/RPoem) / [RCosmo](https://github.com/aon-co-jp/RCosmo) — サーバー側実行基盤(このリポジトリの傘下ではないが両輪をなす)。「分身の術」構成の対象
- [aruaru-llm](https://github.com/aon-co-jp/aruaru-llm) / [open-cuda](https://github.com/aon-co-jp/open-cuda) — 契約不要の独自AI SET構成
