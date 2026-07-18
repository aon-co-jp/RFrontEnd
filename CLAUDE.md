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
  `RHTML::Node`への実適用に変換するアダプタ。RHTML↔RCSS↔RReactの
  「DOM木構築→スタイル解決→VNode化」(2026-07-18)に加え、`Patch`の
  実DOM(`rhtml5::Node`)への反映(`dom_bridge::apply_patch`、
  2026-07-19)まで接続済み。コンポーネントモデル(hooks相当)は
  引き続き未着手。

## 現状(2026-07-18更新)

- RHTML: トークナイザ+DOM木構築器、13テストgreen。
- RCSS: セレクタ/パーサー/カスケード計算+子孫結合子・子結合子(`>`)・
  隣接兄弟結合子(`+`)・一般兄弟結合子(`~`)対応(`~`は2026-07-19追加)、
  32テストgreen。`+`/`~`とも、セレクタの最も右側の結合でのみ判定可能
  という同一のスコープ限界あり(祖先の兄弟情報が呼び出し側から渡され
  ない設計のため、詳細は`RCSS`側CLAUDE.md参照)。
- RJSON: 旧Rust-JSONの内容をそのまま移行(パス依存先の
  `audiocafe-tokyo-server`・`aruaru-db`も更新済み)。
- RReact: 仮想DOM+差分計算(reconciliation)、10テストgreen。加えて
  `dom_bridge`フィーチャ(既定では無効)でRHTML/RCSSと接続する
  End-to-Endパイプライン(RHTMLでDOM木構築→RCSSでスタイル解決→
  RReactのVNode木変換→`Patch`の`rhtml5::Node`への実適用まで)を実装、
  フィーチャ有効時は22テストgreen(2026-07-19、`apply_patch`で
  挿入/削除/置換/属性更新/テキスト更新/keyed子要素の並べ替えに対応、
  「実DOM」はweb-sysのブラウザDOMではなくRHTMLの`Node`木)。
  コンポーネントモデル(hooks相当)は引き続き未着手。
- RTypeScript: 最小スコープ(単純な型注釈の削除・JSへの
  トランスパイルのみ)で新規実装、14テストgreen。GitHubリポジトリ
  `aon-co-jp/RTypeScript`へpush済み。
- RBootStrap: グリッドシステム(container/row/col-*)+基本コンポーネント
  (button/card/navbar)のクラス名パーサー、22テストgreen。
  `aon-co-jp/RBootStrap`へpush済み(2026-07-18、独立リポジトリとして
  未初期化・未pushのまま放置されていたのを巡回中に発見・修正)。
- RGraphQL: v0.2.0スコープ(mutation・変数定義・フラグメント・
  ディレクティブ)に加え、バリデーション層の第一段(未使用フラグメント・
  フラグメント循環・未定義フラグメントspread・重複/匿名オペレーション名・
  未宣言/未使用変数・フィールドセレクションマージの実用的サブセット)を
  実装、49テストgreen。実行エンジン(resolver)は引き続き未着手。
  依存クレートは引き続きゼロ。`aon-co-jp/RGraphQL`へpush済み。
- RNode.js: CommonJS解決(実I/O版含む)・イベントループ・
  `TokioDriver`(実時間駆動)・`fs`コアモジュール相当(2026-07-19追加、
  下記参照)、既定42テスト・全feature有効時46テストgreen。**実バグ発見・
  修正(2026-07-18)**: `TokioDriver::wait_until`が`Handle::block_on`経由で
  永久にハングする不具合があり、`std::thread::sleep`ベースの実装に
  修正(合わせてtokio依存自体を撤去、ゼロ依存を維持)。
  **2026-07-19追記**: 以前ここに「`fs`は実装済み」という前提の記載が
  あったが誤りだった(実際には`resolve.rs`のモジュール解決用
  `StdFileSystem`が存在判定のみを行っていただけで、汎用の`fs`読み書き
  APIは存在しなかった)。読み直しで確認の上、`fs`モジュール
  (`read_file`/`read_to_string`/`write_file`/`exists`/`mkdir`
  (`_recursive`)/`read_dir`/`stat`/`remove_file`/`remove_dir`
  (`_all`)、すべて`std::fs`による実I/O、一時ディレクトリでの
  読み書き往復テストで検証済み)を新規実装し、依存クレートゼロを
  維持したまま`aon-co-jp/RNode.js`へpush済み。

## 運用ルール追記(2026-07-18、正本はopen-raid-zのCLAUDE.md参照) — 確認不要の自動継続・リミット解除後の自動再開

- **コンテキストウインドウ・5時間利用制限・その他のセッション中断が
  発生し、その後リミットが解除されて新しいセッションが開始された場合、
  「続けてよろしいですか」等の確認を挟まず、毎回自動的に前回セッションの
  続きの作業を再開すること**(ユーザー指示、2026-07-18)。具体的には:
  1. セッション開始時、各リポジトリの`git status`/`git log`と、この
     `CLAUDE.md`(および他プロジェクトのCLAUDE.md)のHANDOFF節・
     「次にすべきこと」記載を確認し、未完了・未pushの作業が無いかを
     まず裏取りする(タスク管理メタデータを鵜呑みにしない既存方針と
     同じ姿勢で、実際のgit状態を確認する)。
  2. 未完了作業が見つかった場合、ユーザーへの確認を求めず、そのまま
     自動的に検証(build/test)→修正→コミット→pushまで完了させる。
  3. 完了している場合は、各CLAUDE.mdの「次にすべきこと」「未着手・
     未完成」に記載された次の項目へ確認なしに着手する(既存の
     「未着手だからといって確認を求めて手を止めない」方針の延長)。
  4. 「続けてよろしければそのまま自動開発を継続します」のような、
     続行そのものを尋ねる確認は今後一切行わない(ユーザー指示、
     2026-07-18)。作業内容の要約・進捗報告はしてよいが、それは
     承認を求めるものではなく完了報告として書く。
  5. こまめにコミット・pushしておくことで、次回セッションが「どこから
     再開すべきか」を迷わず`git log`/CLAUDE.mdから機械的に判断できる
     ようにしておく(区切りがついた時点で都度コミット・pushする既存
     方針との組み合わせ)。


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
5. RCSSの`@media`/`!important`対応(子結合子・隣接兄弟結合子・
   一般兄弟結合子(`~`)は2026-07-19までに対応済み)
6. RGraphQLの実行エンジン(resolver、バリデーション第一段は2026-07-19に実装済み)
7. RNode.jsのコアモジュール追加実I/O実装(`fs`は2026-07-19に実装済み
   ——上記「現状」参照。残るは`http`(最小クライアント、実ソケット
   I/O)・`path`(`resolve.rs`内にアドホックに存在する`join`/`normalize`
   を汎用モジュールとして切り出し)等)

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
