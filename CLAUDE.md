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

**2026-09-13 ネーミング統一**: `aruaru.pro`(コンコナラ型スキル
マーケットプレイス+求人サイト統合の新規プロジェクト)向けのフロント
基盤整備に合わせ、`RJSON`→`RS-JSON`へ統合(重複cloneだった
`RFrontEnd\RJSON`を削除し、既存の独立リポジトリ`RS-JSON`を正とした)・
`RTypeScript`→`RS-TypeScript`・`RBootstrap`→`RS-BootStrap`へGitHub
repo rename・crate改称、`RS-JavaScript`(JavaScript自体のトークナイザ/
パーサー/実行エンジン、RS-TypeScriptとは別)を新設した。作業ドライブも
`F:\open-runo`→`F:\runo\repository`へ移行済み(下表のローカル列を更新)。

| プロジェクト | 役割 | GitHub | ローカル | VPS |
|---|---|---|---|---|
| RS-HTML | HTML5相当のブラウザエンジン級パーサー/DOM実装。旧`RTHML`(GitHub上のtypo、ローカルフォルダ名は`RHTML`) | [RS-HTML](https://github.com/aon-co-jp/RS-HTML) | `F:\runo\repository\RFrontEnd\RS-HTML` | `/root/RFrontEnd/RHTML`(旧パスのまま要更新) |
| RS-CSS | CSS3相当のパーサー/カスケード/スタイル計算。旧`RCSS` | [RS-CSS](https://github.com/aon-co-jp/RS-CSS) | `F:\runo\repository\RFrontEnd\RS-CSS` | `/root/RFrontEnd/RCSS`(旧パスのまま要更新) |
| RS-BootStrap | Bootstrap相当のCSSフレームワーク(グリッド+基本コンポーネント。旧`RBootstrap`) | [RS-BootStrap](https://github.com/aon-co-jp/RS-BootStrap) | `F:\runo\repository\RFrontEnd\RS-BootStrap` | `/root/RFrontEnd/RS-BootStrap`(旧パスのまま要更新) |
| RS-TypeScript | TypeScript相当(型注釈削除トランスパイラ、クライアント側ロジック。旧`RTypeScript`) | [RS-TypeScript](https://github.com/aon-co-jp/RS-TypeScript) | `F:\runo\repository\RFrontEnd\RS-TypeScript` | `/root/RFrontEnd/RS-TypeScript`(旧パスのまま要更新) |
| RS-JavaScript | JavaScript(ECMAScript仕様)自体のトークナイザ/パーサー/実行エンジン(2026-09-13新設、ひな形のみ) | [RS-JavaScript](https://github.com/aon-co-jp/RS-JavaScript) | `F:\runo\repository\RFrontEnd\RS-JavaScript` | 未配置 |
| RJSON | JSON処理(旧`Rust-JSON`。**2026-09-13、`RFrontEnd`傘下の重複clone扱いをやめ`RS-JSON`へ統合**——実体は`F:\runo`直下のRS-JSON参照) | [RS-JSON](https://github.com/aon-co-jp/RS-JSON) | `F:\runo\repository\RS-JSON`(`RFrontEnd`傘下ではない) | `/root/RS-JSON` |
| RS-React | React(React DOM/Native/Mobile相当)のコンポーネントモデル。2026-09-13、関数コンポーネント+`use_state`フック→ツリー全体の再レンダーループ(`App`)を実装、続けて同日`RReact`から改称(crate名も`rs-react`へ) | [RS-React](https://github.com/aon-co-jp/RS-React) | `F:\runo\repository\RFrontEnd\RS-React` | `/root/RFrontEnd/RReact`(旧パスのまま要更新) |
| RS-GraphQL | GraphQL仕様の自前実装(トークナイザ/パーサー、v0.2.0でmutation/変数/フラグメント対応)。旧`RGraphQL` | [RS-GraphQL](https://github.com/aon-co-jp/RS-GraphQL) | `F:\runo\repository\RFrontEnd\RS-GraphQL` | `/root/RFrontEnd/RGraphQL`(旧パスのまま要更新) |
| RS-Node.js | Node.jsコア概念の自前実装(CommonJS解決・イベントループ・実行時ドライバ)。旧`RNode.js` | [RS-Node.js](https://github.com/aon-co-jp/RS-Node.js) | `F:\runo\repository\RFrontEnd\RS-Node.js` | `/root/RFrontEnd/RNode.js`(旧パスのまま要更新) |

**2026-09-13 補足**: 上記8リポジトリ中、crate名まで改称したのは
`RS-TypeScript`(`rtypescript`→`rs-typescript`)・`RS-BootStrap`
(`rbootstrap`→`rs-bootstrap`)・`RS-React`(`rreact`→`rs-react`)の3件のみ。
`RS-HTML`(`rhtml5`)・`RS-CSS`(`rcss3`)・`RS-GraphQL`(`rgraphql`)・
`RS-Node.js`(`rnodejs`)はGitHubリポジトリ名・ローカルフォルダ名・
`repository=`フィールドのみ更新し、crate名自体は変更していない
(依存関係の広さ・ソースコード内`use rhtml5::...`等の参照箇所が多く、
名前変更の効果に対してリスクが大きいと判断したため)。

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

## HANDOFF追記(2026-07-19、audiocafe-tokyo-rustユーザーからの指示による次回着手事項)

- `audiocafe-tokyo-rust`(RJSON=`rust-json`を実際に使用中の実プロジェクト、
  RTypeScript/RReact等は現状未使用)のユーザーから、エコシステム内の
  各リポジトリについて「未着手や未完成の技術があれば、それぞれの
  リポジトリもTESTしながら完成させていって、実用性と完成度を高めて
  いって下さい」という要望があった。ユーザー確認の上、各リポジトリを
  **別セッションで順に実施**する方針となったため、本セッションでは
  実装は行わず、次回このリポジトリ(またはRTypeScript/RReact/RCSS/
  RHTML/RGraphQL/RNode.js/RBootStrap等の傘下リポジトリ)のセッションが
  開始した際は、上記「次にすべきこと」1〜7を確認不要でそのまま
  着手すること(既存の運用ルール通り)。
  - 傘下リポジトリの中で最も進捗が浅いのは**RBootstrap**
    (2026-07-20、GitHubリポジトリ名を`RBootStrap`→`RBootstrap`に
    ユーザー指示でリネーム済み——ローカル`F:\open-runo\RFrontEnd\
    RBootstrap`・VPS`/root/RFrontEnd/RBootstrap`・GitHub remoteとも
    更新済み、旧名での言及は過去のHANDOFF本文にのみ残る)
    (コミット2件のみ、グリッド+基本コンポーネントのクラス名パーサー
    段階、flexbox折り返し計算・レスポンシブ`@media`出力が未着手)。
    優先度をつけるなら、まずここから着手するのが妥当。

## 関連プロジェクト

- [open-raid-z](https://github.com/aon-co-jp/open-raid-z) — 開発ルールの正本
- [RPoem](https://github.com/aon-co-jp/RPoem) / [RCosmo](https://github.com/aon-co-jp/RCosmo) — サーバー側実行基盤(このリポジトリの傘下ではないが両輪をなす)。「分身の術」構成の対象
- [aruaru-llm](https://github.com/aon-co-jp/aruaru-llm) / [open-cuda](https://github.com/aon-co-jp/open-cuda) — 契約不要の独自AI SET構成
- **aruaru.pro**(2026-09-13着手予定、新規リポジトリ未作成) — コンコナラ型
  スキルマーケットプレイス+求人サイト統合。Rust + RPoem + RS-React軸、
  決済はStripe Connect。フロント基盤としてRS-React(旧`RReact`)の
  コンポーネントモデル(第一段は完了、ツリー全体の再レンダーループが
  次の課題)を使う想定。
  既存の`job-site`(RPoem+aruaru-db、求人サイト試作品、リモート未push、
  `F:\runo\repository\job-site`)とは別件として現状のまま残す方針
  (ユーザー指示、2026-09-13)。

## API 方針: REST 全廃・gRPC / Connect へ (2026-11-04)

### 日本語
- **REST は全廃する。新しい API は REST で作らない。** ブラウザや `curl` から呼ぶ API は **Connect の単純呼び出し**(`POST /<パッケージ>.<Service>/<Method>`、`content-type: application/json`、本文は JSON)、サービス間は **gRPC / gRPC-web**(RPoem の管理API `open_runo.admin.v1.Admin` と同じ流儀)。ストリーミングは、必要になってから。
- **既存の REST は、触るときに順に移す。** 移したら旧 REST の経路は消す。移行が終わるまで残るが、**新規には追加しない**。
- **JSON は RS-JSON に統一**する(読み込みは `from_str_secure` / `parse_secure` でサイズ・深さ・重複キーの上限つき、書き出しは `rust_json::fast`)。ブラウザの WASM に `prost` は入れない(サイズが増えるため)。
- **例外(自分たちの API ではなく、相手の規格が決めているもの)**: ACME、OAuth/OIDC のコールバック、第三者(GitHub など)からの Webhook、ヘルスチェック、静的ファイル配信、OTLP など。これらは規格どおりの HTTP を受ける。
- 参考実装: `RPoem`(`open_runo.admin.v1.Admin`)、`open-easy-web`(`server/src/connect.rs`、`proto/open_easy_web/admin/v1/admin.proto`)。
- ※ このリポジトリのコードが既に方針どおりかどうかは、各リポジトリの現状による。この節は方針の記録であり、コードの変更を意味しない。

### English
- **REST is being abolished. New APIs are not built as REST.** APIs called from browsers or `curl` use **Connect unary calls** (`POST /<package>.<Service>/<Method>`, `content-type: application/json`, JSON body); service-to-service calls use **gRPC / gRPC-web** (the same style as RPoem's admin API `open_runo.admin.v1.Admin`). Streaming only when it is actually needed.
- **Existing REST endpoints are migrated one by one, when they are touched.** The old REST route is removed once migrated. Until migration is finished they remain, but **no new ones are added**.
- **JSON is unified on RS-JSON**: reading with `from_str_secure` / `parse_secure` (size, depth and duplicate-key limits), writing with `rust_json::fast`. `prost` is not put into browser WASM (it would grow the binary).
- **Exceptions (shapes dictated by someone else's standard, not our own API)**: ACME, OAuth/OIDC callbacks, webhooks from third parties (e.g. GitHub), health checks, static file serving, OTLP, and so on. These accept plain HTTP as the standard requires.
- Reference implementations: `RPoem` (`open_runo.admin.v1.Admin`) and `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Note: whether the code in a given repository already follows this policy depends on that repository. This section records the policy; it does not mean the code has been changed.

### Deutsch (KI-Übersetzung, redaktionell überarbeitet; noch nicht von Muttersprachlern geprüft)
- **REST wird vollständig abgeschafft; neue APIs entstehen nicht mehr als REST.** APIs, die aus dem Browser oder per `curl` aufgerufen werden, verwenden **unäre Connect-Aufrufe** (`POST /<Paket>.<Service>/<Methode>`, `content-type: application/json`, JSON-Body). Zwischen Diensten kommen **gRPC bzw. gRPC-web** zum Einsatz, im selben Stil wie die Admin-API von RPoem (`open_runo.admin.v1.Admin`). Streaming wird erst ergänzt, wenn es tatsächlich gebraucht wird.
- **Bestehende REST-Endpunkte werden schrittweise umgestellt, und zwar immer dann, wenn man ohnehin an ihnen arbeitet.** Sobald ein Endpunkt umgestellt ist, wird die alte REST-Route entfernt. Bis zum Abschluss der Migration bleiben die übrigen bestehen, es werden aber **keine neuen mehr hinzugefügt**.
- **JSON wird einheitlich über RS-JSON verarbeitet**: Gelesen wird mit `from_str_secure` bzw. `parse_secure` (mit Obergrenzen für Größe, Verschachtelungstiefe und doppelte Schlüssel), geschrieben mit `rust_json::fast`. `prost` wird nicht in das Browser-WASM aufgenommen, da es die Datei vergrößern würde.
- **Ausnahmen (Schnittstellen, deren Form ein fremder Standard vorgibt und die nicht zu unserer eigenen API gehören)**: ACME, OAuth/OIDC-Callbacks, Webhooks von Drittanbietern (z. B. GitHub), Health-Checks, die Auslieferung statischer Dateien, OTLP und Ähnliches. Diese nehmen, wie vom jeweiligen Standard verlangt, gewöhnliches HTTP entgegen.
- Referenzimplementierungen: `RPoem` (`open_runo.admin.v1.Admin`) und `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Hinweis: Ob der Code eines einzelnen Repositorys diese Richtlinie bereits erfüllt, hängt vom jeweiligen Repository ab. Dieser Abschnitt dokumentiert lediglich die Richtlinie; er besagt nicht, dass der Code schon angepasst wurde.

### Русский (перевод выполнен с помощью ИИ и отредактирован; носителями языка пока не проверен)
- **От REST мы полностью отказываемся; новые API больше не создаются в стиле REST.** API, к которым обращаются из браузера или через `curl`, используют **унарные вызовы Connect** (`POST /<пакет>.<Service>/<Method>`, `content-type: application/json`, тело запроса — JSON). Для взаимодействия между сервисами применяются **gRPC и gRPC-web** — в том же стиле, что и административный API RPoem (`open_runo.admin.v1.Admin`). Потоковая передача данных добавляется лишь тогда, когда в ней действительно возникает необходимость.
- **Существующие REST-эндпоинты переводятся поэтапно — по мере того, как с ними приходится работать.** После перевода старый REST-маршрут удаляется. Пока миграция не завершена, оставшиеся эндпоинты сохраняются, но **новые не добавляются**.
- **Работа с JSON унифицирована на основе RS-JSON**: чтение — через `from_str_secure` или `parse_secure` (с ограничениями на размер, глубину вложенности и повторяющиеся ключи), запись — через `rust_json::fast`. `prost` в браузерный WASM не включается, поскольку это увеличило бы размер файла.
- **Исключения (интерфейсы, форму которых определяет чужой стандарт, а не наш собственный API)**: ACME, обратные вызовы OAuth/OIDC, вебхуки сторонних сервисов (например, GitHub), проверки работоспособности, раздача статических файлов, OTLP и тому подобное. Такие интерфейсы принимают обычный HTTP, как того требует соответствующий стандарт.
- Эталонные реализации: `RPoem` (`open_runo.admin.v1.Admin`) и `open-easy-web` (`server/src/connect.rs`, `proto/open_easy_web/admin/v1/admin.proto`).
- Примечание: соответствует ли код конкретного репозитория этой политике, зависит от самого репозитория. Данный раздел лишь фиксирует политику и не означает, что код уже изменён.

## API 方針の補足: 用語の整理と使い分け (2026-11-04)

### 日本語
**1つに統一するものではなく、別の層の話です。** 「REST の代わり」になるのは Connect・gRPC・GraphQL だけで、WASM は画面を作る技術です。

| 用語 | 何か | REST の代わりか |
|---|---|---|
| REST | URL と HTTP メソッドで操作を表す API の作り方 | 全廃する側 |
| **Connect** | `POST /サービス名/メソッド名` に JSON を送る形式。`curl` でもブラウザの `fetch` でも呼べる | はい(**採用**) |
| **gRPC** | 同じ「サービス/メソッド」の考え方を、バイナリ(protobuf)と HTTP/2 で行う形式。速く、ストリーミングも得意 | はい(**サービス同士に採用**) |
| gRPC-web | gRPC のブラウザ向けの変形版 | はい(必要なら) |
| GraphQL | 欲しい項目を問い合わせる形式 | 一部だけ(**問い合わせ(読み取り)専用**) |
| WASM | Rust をブラウザの中で動かす仕組み | いいえ(画面を作る技術) |

- **ブラウザの画面や `curl` から呼ぶ API は Connect**(POST、JSON を RS-JSON で読み書き)。ブラウザの WASM は素の gRPC を話せず、Connect なら `fetch` のままで、`prost` を足さずに済む(画面のサイズも増えない)。
- **サービス同士の通信(サーバー間、重い処理、ストリーミング)は gRPC。** 多くは Connect で足りるので、ストリーミングが必要になった所だけ gRPC に上げる。
- **GraphQL は、まとめて問い合わせたいときの読み取り専用。** 書き込み(操作)には使わない。
- **WASM は画面を作る技術として、そのまま使う。** 画面から呼ぶ API が Connect になっただけ。
- 外部の規格が形を決めるもの(ACME、OAuth のコールバック、第三者の Webhook、ヘルスチェック、静的ファイル、OTLP)は、規格どおりの HTTP のまま。
- **迷ったら Connect。**

### English
**These are not alternatives to unify; they sit at different layers.** Only Connect, gRPC and GraphQL can replace REST; WASM is a way to build the UI.

| Term | What it is | Replaces REST? |
|---|---|---|
| REST | API style that expresses operations through URLs and HTTP methods | The thing being abolished |
| **Connect** | `POST /Service/Method` with a JSON body; callable from `curl` and from a browser `fetch` | Yes (**adopted**) |
| **gRPC** | The same service/method idea over binary (protobuf) and HTTP/2; fast, good at streaming | Yes (**for service-to-service**) |
| gRPC-web | A browser-friendly variant of gRPC | Yes (if needed) |
| GraphQL | A query language: ask for exactly the fields you want | Partly (**read-only queries**) |
| WASM | Runs Rust inside the browser | No (a UI technology) |

- **APIs called from browser screens or `curl` use Connect** (POST, JSON read and written with RS-JSON). Browser WASM cannot speak plain gRPC; with Connect it stays a `fetch`, and `prost` is not needed (the binary does not grow).
- **Service-to-service calls (server to server, heavy work, streaming) use gRPC.** Connect is enough in most places; move a call to gRPC only where streaming is actually needed.
- **GraphQL is read-only**, for queries that gather several pieces of data. It is not used for writes (operations).
- **WASM stays as the way to build the UI.** The API it calls simply became Connect.
- Interfaces whose shape an outside standard dictates (ACME, OAuth callbacks, third-party webhooks, health checks, static files, OTLP) stay plain HTTP.
- **When in doubt, use Connect.**

### Deutsch (KI-Übersetzung, sprachlich überarbeitet; noch nicht von Muttersprachlern geprüft)
**Es geht nicht darum, eine einzige Technik auszuwählen – die Begriffe liegen auf unterschiedlichen Ebenen.** Als Ersatz für REST kommen nur Connect, gRPC und GraphQL in Frage; WASM dagegen ist eine Technik, mit der die Benutzeroberfläche gebaut wird.

| Begriff | Was ist das? | Ersatz für REST? |
|---|---|---|
| REST | Ein API-Stil, bei dem Operationen über URLs und HTTP-Methoden ausgedrückt werden | wird abgeschafft |
| **Connect** | `POST /Dienst/Methode` mit JSON im Body; aufrufbar mit `curl` ebenso wie mit `fetch` im Browser | ja (**gewählt**) |
| **gRPC** | Dasselbe Prinzip aus Dienst und Methode, aber über binäres protobuf und HTTP/2; schnell und gut für Streaming geeignet | ja (**zwischen Diensten**) |
| gRPC-web | Eine für den Browser angepasste Variante von gRPC | ja (bei Bedarf) |
| GraphQL | Abfragesprache, mit der man genau die benötigten Felder anfordert | teilweise (**nur für lesende Abfragen**) |
| WASM | Führt Rust-Code direkt im Browser aus | nein (Technik für die Oberfläche) |

- **APIs, die von Browseroberflächen oder per `curl` aufgerufen werden, verwenden Connect** (POST; JSON wird mit RS-JSON gelesen und geschrieben). WASM im Browser kann kein reines gRPC sprechen. Mit Connect bleibt es bei einem gewöhnlichen `fetch`, und `prost` ist nicht nötig – die WASM-Datei wird dadurch nicht größer.
- **Für die Kommunikation zwischen Diensten (Server zu Server, rechenintensive Aufgaben, Streaming) wird gRPC eingesetzt.** In den meisten Fällen reicht Connect aus; auf gRPC wird nur dort umgestellt, wo Streaming tatsächlich gebraucht wird.
- **GraphQL dient ausschließlich lesenden Abfragen**, wenn mehrere Informationen gebündelt abgefragt werden sollen. Für Schreibzugriffe (Operationen) wird es nicht verwendet.
- **WASM bleibt die Technik für die Oberfläche.** Geändert hat sich lediglich, dass die aufgerufene API nun auf Connect beruht.
- Schnittstellen, deren Form durch einen externen Standard festgelegt ist (ACME, OAuth-Callbacks, Webhooks von Drittanbietern, Health-Checks, statische Dateien, OTLP), bleiben gewöhnliches HTTP.
- **Im Zweifel: Connect.**

### Русский (перевод выполнен с помощью ИИ и литературно отредактирован; носителями языка пока не проверен)
**Это не варианты, из которых нужно выбрать один, а технологии разных уровней.** Заменить REST могут только Connect, gRPC и GraphQL; WASM же — это способ создания пользовательского интерфейса.

| Термин | Что это | Заменяет REST? |
|---|---|---|
| REST | Стиль построения API, при котором операции выражаются через URL и HTTP-методы | отменяется |
| **Connect** | `POST /Сервис/Метод` с телом в формате JSON; вызывается как из `curl`, так и из браузерного `fetch` | да (**выбран**) |
| **gRPC** | Тот же принцип «сервис — метод», но поверх двоичного формата protobuf и HTTP/2; работает быстро и хорошо подходит для потоковой передачи | да (**для связи между сервисами**) |
| gRPC-web | Адаптированный для браузера вариант gRPC | да (при необходимости) |
| GraphQL | Язык запросов: клиент запрашивает ровно те поля, которые ему нужны | частично (**только запросы на чтение**) |
| WASM | Позволяет выполнять код на Rust прямо в браузере | нет (технология для интерфейса) |

- **API, которые вызываются из браузерных интерфейсов или через `curl`, строятся на Connect** (POST; JSON читается и записывается через RS-JSON). Браузерный WASM не умеет работать с «чистым» gRPC. С Connect достаточно обычного `fetch`, а `prost` не требуется — размер WASM-файла не увеличивается.
- **Для обмена между сервисами (сервер — сервер, ресурсоёмкие задачи, потоковая передача) применяется gRPC.** В большинстве случаев достаточно Connect; на gRPC переходят только там, где потоковая передача действительно необходима.
- **GraphQL используется только для чтения**, когда нужно получить сразу несколько связанных данных. Для записи (операций) он не применяется.
- **WASM по-прежнему остаётся технологией для интерфейса.** Изменилось лишь то, что вызываемый им API теперь построен на Connect.
- Интерфейсы, форма которых задана внешним стандартом (ACME, обратные вызовы OAuth, вебхуки сторонних сервисов, проверки работоспособности, статические файлы, OTLP), остаются обычным HTTP.
- **Если есть сомнения — выбирайте Connect.**
