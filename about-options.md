# 知っておくとよいoption
- `--cache_checked_modules` 

つけておくと，成功した型検査の結果がキャッシュされる．とりあえず付けておくとよい．
- `--codegen` `--odir`

自分がOCamlコードを吐き出させる際に使っているオプション．詳細は割愛．

- `--detail_errors`

エラー箇所が細かく出る．(しかし，型検査時間がとても増えるし，たまに間違ったところでエラーが出るので，私はあまり信用していない)

- `--fuel` `--ifuel` など

関数定義，もしくは型定義の展開回数を指定するためのオプション．
fuel 系は再帰関数の展開回数を指定する．ifuel系は型定義の展開回数を指定する．

たとえば，`assert (FStar.List.length ([0;0;0;0;0;0;0;0;0]) = 9)` というassertationは現状のfstarコンパイラでは検査に失敗する．これは，再帰関数の展開回数上限(`--max-fuel`)がデフォルトでは8になっているためである．
`--fuel 10`というオプションを付けるとfstarコンパイラが型検査に成功するようになる．

詳しくは https://github.com/FStarLang/FStar/wiki/Using-SMT-fuel-and-the-normalizer を参照．

- `--lax`

篩型部分を無視した型検査を行う．
ソースコード中で`#set-options "--lax"`などを
用いることにより，検証中の部分以外にかかる検査時間を短縮することができる．

- `--z3rlimit`

型検査時のz3の稼働時間を指定できる．複雑な定理の証明時にはこれを増やさないと型検査に失敗することがある．なぜか型検査に落ちる場合はとりあえずこの値を増やしてみるとよい．

# set-optionsまわり
実装は`src/basic/FStar.Options.fs`で行われている．

https://github.com/FStarLang/FStar/wiki/Pragmas-%28%23set-options%2C-%23reset-options%29
ではよく分からなかったので実験．

- 実験コード
```
(* from https://github.com/FStarLang/FStar/wiki/Using-SMT-fuel-and-the-normalizer *)
let _ =
  assert (FStar.List.length ([0] @ [0;0;0] @ [0;0;0;0;0]) = 9)
```
`--fuel 10` でないと証明に失敗する．

- `#set-options`

optionを追加する．
- `#reset-options`

set-optionsで追加されたoptionを消す．(そして，オプションを追加する)(コマンドラインから渡した引数は残る)

- `#push-options`
内部的には push-stack -> set-options をやっている

- `#pop-options`

stackから状態を復元する

(コードを読むと`Options.internal_push `が呼ばれているが，これは先頭しかいじっていない気がするのだけれどどうなんだろ...)