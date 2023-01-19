Medium好讀版：[點這裡](https://medium.com/@mh.yang/ffmpeg-adelay%E7%9A%84%E5%96%AE%E4%BD%8D%E5%8F%8A%E5%B0%8D%E9%BD%8A%E7%9A%84%E6%A8%99%E7%9A%84-bcd836976cd6)

### 前言
接續前一篇：[ffmpeg依不同offset合併多個音檔](https://medium.com/@mh.yang/ffmpeg%E4%BE%9D%E4%B8%8D%E5%90%8Coffset%E5%90%88%E4%BD%B5%E5%A4%9A%E5%80%8B%E9%9F%B3%E6%AA%94-ac13ff2fb50c)

在開發時有2個不確定的問題：

1. timestamp是對齊誰？

2. adelay單位是秒還是毫秒。就做了個小實驗來驗證。

### 問題描述

更具體來説，上述的2個問題：

1. timestamp是對齊誰？

- 是所有音檔都與第一個音檔算offset呢？(以下簡稱FIRST)還是彼此之間算offset (以下簡稱BETWEEN)。

2. adelay的單位是秒還是毫秒？

- 根據ffmpeg的手冊，是用毫秒沒錯。

> *8.11 adelay*
> *Delay one or more audio channels.*
> 
> *Samples in delayed channel are filled with silence. The filter accepts the following option:*
> 
> *delays Set list of delays in milliseconds for each channel separated by ’|’.*

### 實驗
接下來就是要驗證對齊的目標，實驗設定我取了3個音檔，分別是RE、FA、LA這三個音。

彼此的長度都是5秒，5秒過後就換下一個聲音。因此預期的結果應該不會有和聲。

![音檔排列示意圖](https://miro.medium.com/max/720/1*WKniQ4Xuz-efRxZllnamLA.png)


所使用的2組設定分別爲：

- 方法1 FIRST
```sh
## 方法1 FIRST: 所有音檔都跟第一筆算offset
fmpeg -i re.mp3 -i fa.mp3 -i la.mp3 -filter_complex \
“[1]adelay=5000[a1];
 [2]adelay=10000[a2];
 [0][a1][a2]amix=3:normalize=0” misec_first.mp3
```

- 方法2 BETWEEN

```sh
## 方法2 BETWEEN: 所有音檔彼此跟前一筆算offset
fmpeg -i re.mp3 -i fa.mp3 -i la.mp3 -filter_complex \
"[1]adelay=5000[a1];
 [2]adelay=5000[a2];
 [0][a1][a2]amix=3:normalize=0" misec_first.mp3
 ```

### 實驗結果

從音檔的分佈可以看出，依序更換，沒有重叠的FIRST才是正確的方法：

- FIRST的方法

![FIRST的結果](https://miro.medium.com/max/720/1*FPsL5t0isqNxAru69SOtdg.png)

- BETWEEN的方法

![BETWEEN的結果](https://miro.medium.com/max/720/1*G3qWm4oGmhRDFh_I-vTPMA.png)


### 結論

ffmpeg的adelay，計算offset是以第一筆音檔爲主 (大家都要跟第一筆算offset)，

而單位是用millisecond。
