# forallの導入
```
val p : int -> Type0
val proof : x:int -> Lemma (p x)
```
があるときに， `Classical.forall_intro` を用いることで `forall x. p x` を示すことができる．

以下のようにすればよい．
```
let _ = 
	Classical.forall_intro #int #p proof;
	assert (forall x. p x)
```
