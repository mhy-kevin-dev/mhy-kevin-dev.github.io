## h5py tutorial:
https://www.getdatajoy.com/learn/Read_and_Write_HDF5_from_Python

`import h5py`

### Create:

```py
h5f = h5py.File("output_dir/tr_XY.h5", "w")
h5f.create_dataset("_feats_", data=tr_X, compression=True)
h5f.create_dataset("_label_", data=tr_Y, compression=True)
h5f.close()
```

### Load:

```py
h5f = h5py.File("output_dir/tr_XY.h5", "r")
tr_X = h5f["_feats_"]
tr_Y = h5f["_labels_"]
```

或者
`with h5py.File('output_dir/tr_XY.h5', "w") as hf:`



## 其它存檔方式的缺點:

### numpy.save 、 numpy.savez 和 scipy.io.savemat

- numpy和scipy提供的資料儲存方法。但是實際上這三個方法產生的文件大小都是一樣的非常大。 

- 8000張256*256*3的圖片出來就是一個16G的檔案。

### cPickle 搭配 gzip

- pkl.gz 是mnist官方提供的檔案。看來是會很好用的樣子。 但是實際使用中，有兩個難以避免的問題:  (1)速度慢，記憶體佔用量高（效能不好）; (2)大矩陣儲存無能
- 關於後者，這是python官方bug，如果你在cPickle.dump()的時候遇到“ SystemError: error return without exception set”，那麼恭喜你中獎了(python3以上修好了， python2.7還有bug)。
- 儘管cPickle+gzip性能已經很優秀，但是和h5py性能的對比還是比較差
 

### Reference

http://www.shocksolution.com/2010/01/storing-large-numpy-arrays-on-disk-python-pickle-vs-hdf5adsf
