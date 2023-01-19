### baseline LM訓練指令


### requirements

- Chinese Gigaword Dataset (the CNA part)

- Kaldi

- MITLM  (安裝步驟看[這篇](https://mhy-kevin-dev.github.io/install-MITLM/))

### 流程

1. 算count
 
```sh
gunzip -c cna.split.txt.gz  | cut -d' ' -f2- | ngram-count -text /dev/stdin -write count.split.txt.gz
```
 
2. Smoothing
 
```sh
~/MITLM/bin/estimate-ngram -c count.split.txt.gz -o 3 -s ModKN -wl lm0.arpa.gz
```
 
3. Pruning+砍掉OOV

(tmp_vocab是字典的第一個column)

```sh
ngram -debug 1 -order 3 -lm lm0.arpa.gz -vocab tmp_vocab -limit-vocab -prune 1e-7 -prune-lowprobs -unk -renorm -write-lm lang_test/lm.gz
```

 
4. 把LM變成G.fst 

(在lang_test底下)

```sh
gunzip -c pruned.arpa.gz | arpa2fst - - | fstprint | eps2disambig.pl | s2eps.pl | fstcompile --isymbols="words.txt" --osymbols="words.txt" --keep_isymbols=false --keep_osymbols=false | fstrmepsilon | fstdeterminizestar --use-log=true | fstarcsort --sort_type=ilabel > "G.fst"
```
