# 検証内容の書き方
## 引数かforallか


たとえば， `List.Tot.append_mem` と `List.Tot.append_mem_forall` の関係

a)
```
val f : a:int -> b:int -> Lemma (a + b = b + a)
```

- 引数の明示的適用ができる

b)

```
val f : a:int -> Lemma (forall b. a + b = b + a)
``` 

- この形でないと使えない定理がある(たとえば，`List.Tot.mem_filter_forall`)

## 篩型かpre-postconditionか

a)
```
val f : a:int{a > 0} -> r:int{r = a + 1}
```

b)

```
val f : a:int -> Pure int (requires (a > 0)) (ensures (fun r -> r = a + 1))
``` 

- エフェクトがMLなどの場合，こちらで無いとメモリとの関係が書けない
