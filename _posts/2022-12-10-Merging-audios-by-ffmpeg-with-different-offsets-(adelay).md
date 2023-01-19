# ffmpeg使用adelay合併多個音檔

### 前言
最近進行會議錄音處理時，有個功能需要根據會議ID，到DB撈出與會人員的錄音和當時的辨識結果，依照DB記錄的timestamp合成一份完整的會議錄音和字幕檔。

### 環境
由於ffmpeg的amix會自動normalize音量 (n個音檔一起合的話，音量就會變1/n)，若一次取太多個音檔，會導致最後合出來的音檔太小聲。

ffmpeg 5.1以上，才可用normalize:0取消自動normalize，建議到官網下載tarball安裝，因為apt-get安裝的版本太舊。

此外，使用的音檔是單聲道，16kHz。

### 合成
假設現在有四個音檔，它們各自有開始時間。

- 1.mp3是最一開始的音檔。
- 2.mp3的開始時間，距離1.mp3的開始時間是12秒。
- 3.mp3的開始時間，距離1.mp3的開始時間是18秒。
- 最後4.mp3的開始時間，距離1.mp3的開始時間是15秒。

音檔的相對位置畫成圖片長這樣：
![這四個音檔的相對位置](https://miro.medium.com/max/720/1*5v3K1WIFnXEp7UCVxbCt5Q.png)

這時指令可以這樣寫：
```sh
ffmpeg -i 1.mp3 -i 2.mp3 -i 3.mp3 -i 4.mp3 -filter_complex 
"[1]adelay=12000[a1];
  [2]adelay=18000[a2];
  [3]adelay=15000[a3];
  [0][a1][a2][a3]amix=4:normalize=0" output.mp3
```
adelay的時間單位是：millisecond。
normalize=0：取消自動normalize音量。

### 實務
實際上，線上會議一開始一定會有一段silence。
因此合併時通常會拿一個無聲的短音檔假裝是1.mp3，它的開始時間就是會議開始的時間，然後其他人的音檔再依序去對齊它就好了。

### Follow-up
不過，後來發現即使照著DB記錄的timestamp去合成，聲音仍會出現overlap的狀況(即使開會途中沒有撞話)。
可能因為線上會議有網路延遲，導致音檔內容和timestamp有誤差。
除非server的規格更新，每個封包都標記起始的timestamp，再依序存檔，否則就只能在事後合併時做一些手腳以減緩影響。
