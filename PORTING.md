# PORTING.md — お引越し可能ファイル

`RFrontEnd`傘下プロジェクト間、および他のRust系エコシステム
(open-raid-z/aruaru-db/open-web-server等)へそのまま(または軽微な
変更で)移植できる実装パターン一覧。

## `TokenSink`パターン(RHTML由来)

トークナイザ(字句解析器)とDOM木構築器を疎結合にする設計。
`TokenSink`トレイトを実装するだけで、トークナイザ側のコードを
一切変更せずに別の消費側(DOM構築・デバッグ用収集器等)へ差し替えられる。

```rust
pub trait TokenSink {
    fn process_token(&mut self, token: Token);
}
```

新しい構文解析器(将来のRTypeScript等)を書く際も、字句解析と
構文木構築を同じパターンで分離しておくと、テスト・デバッグ・
将来の再実装がしやすくなる。

## DOM実装非依存のマッチングトレイト(RCSS由来)

CSSエンジン(`RCSS`)は特定のDOM実装(`RHTML::Element`)に直接
依存せず、`ElementLike`トレイト(`tag_name()`/`classes()`/`id()`)
経由で要素とマッチングする。

```rust
pub trait ElementLike {
    fn tag_name(&self) -> &str;
    fn classes(&self) -> Vec<&str>;
    fn id(&self) -> Option<&str>;
}
```

この設計により、CSSエンジン自体を別のUIツールキット・別のDOM表現
(将来のネイティブUI等)にもそのまま応用できる。「エンジンAとエンジンB
を組み合わせて使いたいが、片方が片方の内部実装に直接依存してしまう」
という結合を避けたい場面全般で応用可能。

## `Patch`ベースの差分表現(RReact由来)

仮想DOMの差分計算結果を「置き換え」「テキスト更新」「要素の
in-place更新(属性・子要素どちらか一方または両方)」という
明示的な列挙型で表現する設計。

```rust
pub enum Patch {
    Replace(VNode),
    UpdateText(String),
    UpdateElement { attrs: Option<AttrsPatch>, children: Option<Vec<ChildPatch>> },
    NoOp,
}
```

「属性差分と子差分の両方がある場合の表現」を後から無理やり
組み合わせるのではなく、最初から複合ケースを持つ専用バリアントとして
設計しておくと、呼び出し側のパターンマッチが素直になる(このリポジトリ
自身が最初の実装で複合ケースの表現に迷い、書き直した実例がある——
`RReact/CLAUDE.md`のHANDOFF参照)。

## `rust_json`の型付き厳密モードAPI(RJSON由来)

`serde_json::from_str::<T>`相当の型付きデシリアライズを、
このエコシステム共通のJSON処理クレート(`RJSON`)経由に統一するための
関数群(`from_slice_strict::<T>`/`from_str_strict::<T>`/
`to_vec_strict`/`to_string_strict`/`to_string_pretty_strict`)。
`serde_json`へそのまま委譲するため出力バイト列は完全に同一——
チェックサムに依存する既存コード(`aruaru-db`のZFS互換チェックサム層等)
への移行時にも安全に使える。
