### decode_denlats時，在log看到錯誤訊息：

Lattice determinization aborted since looped more than 500000 times during epsilon closure

### 目前找到的解決方法有兩種：
	
1. lattice-beam設小一點， 參考資料

- 副作用：可以解決這個問題，雖然速度比較快，但正確率會掉 
 
2. 修改 nnet3/make_denlats.sh
		
- 在max_mem=20000000下面插入一行：max_loop=1000000
- 找到這行(應該是161行)，並改成： 

```sh
lattice_determinize_cmd="lattice-determinize-non-compact --acoustic-scale=$acwt --max-mem=$max_mem --max-loop=$max_loop  --minimize=$minimize --prune=true --beam=$lattice_beam ark:- ark:- |"
```

- 備註：跟max_loop有關的程式碼在 latbinlattice-determinize-non-compact.cc 第217行

### Reference

[[Kaldi-users] issues with decoding (self loops) in tri2a for 1-minute speech segments](https://sourceforge.net/p/kaldi/mailman/message/31116094)

