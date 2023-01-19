Medium好讀版：[點這裡](https://medium.com/@mh.yang/wsl2-vscode-conda-formatter%E4%BD%BF%E7%94%A8%E8%A8%AD%E5%AE%9A-acca390e94c8)

### 前言
自從工作環境從Ubuntu Server移動到WSL2之後，也想試著改變自己原有的Workflow。也看到一些VSCode搭配Formatter自動排版的使用心得，不僅能統一團隊的程式碼風格，也能節省人工排版的額外effort。看起來滿吸引人的，就嘗試設定看看。

![圖1：先在VSCode安裝這個插件](https://miro.medium.com/max/720/1*thFLsR2ysgWUTmHJaeODyw.png)

### 環境設定
在Windows10安裝VSCode。WSL2使用Ubuntu 22.04，並已經先安裝好了Conda，建立一個python 3.9的環境叫py39。

在VSCode，先安裝由Microsoft提供的這個WSL這個插件 (如圖1)，接著點到側邊欄的WSL(安裝完重啟VSCode會出現)，右鍵連進WSL (如圖2)。稍微等一下，會自動重新打開一個視窗，右下角會出現綠色的WSL (如圖3)：

![圖2：點到側邊欄的WSL，右鍵連進WSL](https://miro.medium.com/max/640/1*ueK7P58LmFsJ2wGekY5w2g.png)

![圖3：表示VSCode成功啟動WSL](https://miro.medium.com/max/420/1*hwRbmR6cpxQJvhEaE7q8_w.png)

接著在WSL這個VSCode視窗繼續安裝python插件：Python (如圖4)
![圖4：在WSL的VSCode視窗安裝Python插件](https://miro.medium.com/max/720/1*kZstBaA0o1mp0TSLQSRXvQ.png)

安裝好之後，接下來要設定Conda路徑，到File -> Prereferences -> Settings -> 在搜尋列輸入conda -> 選擇Remote [WSL: Ubuntu-22.04] -> 找到Python: Conda Path -> 填入你在WSL安裝的conda路徑，如下圖5所示。

`/home/你的帳號/miniconda3/bin/conda`

![圖5：到對應的位置設定WSL的Conda路徑](https://miro.medium.com/max/720/1*xzlJ4JEBzy93UDGSu23XaA.png)

接著設定存檔時自動排版，如下圖6所示：

![圖6：設定存檔時自動排版](https://miro.medium.com/max/720/1*LLkjBqiF3QzGEgTqAGNtAw.png)

最後要設定預設的Interpreter，ctrl + shift + p啟動Command Palette -> 輸入python interpreter -> 會讓你選預設要進入的環境 (如圖7)。

![圖7：設定預設的python環境](https://miro.medium.com/max/640/1*4yJExqvpq0iJKOip90j1rg.png)

此時可以打開一個python檔案，寫一些code，按存檔時如果你的python環境沒有安裝formatter，會跳出提醒問你要安裝哪種 (如圖8)，有black, autopep8和yapf三種可以選，選了會問你要用Conda安裝還是pip (如圖9)，然後VSCode會幫你啟動環境，然後下指令自動安裝 (如圖10)。

![圖8：安裝你想要使用的formatter，我是用black](https://miro.medium.com/max/640/1*UoYjNMAutP_kUGh8HTvfbw.png)

![圖9：選擇要用Conda還是pip安裝formatter](https://miro.medium.com/max/640/1*l1gFX6vcJ6rtJr6_mO4bQQ.png)

![圖10：VSCode自動安裝formatter，先前安裝時沒到拍到圖，所以換一個乾净的環境裝autopep8，也可以選其他兩個。](https://miro.medium.com/max/720/1*7pOyP4uOATLYbCTn5l0GQQ.png)

### 測試
設定完成之後就可以測試了，故意寫一段需要排版，而且沒有對齊的python程式。我這邊的formatter是用black。

```python
import    pandas
import            os,sys  # 逗號後面沒有空格

if   __name__=='__main__'   :  #   等號前後沒有空格，單引號雙引號混用
    num =         "1"   #     故意空很多個空格

    if  num.isdigit()   :  #     故意空很多個空格
        print ( 'num is a digit'   )  # 小括號前故意空很多個空格
```

接著按存檔，就會自動幫你排版成統一的格式。

![圖11：存檔後，以black格式做的自動排版](https://miro.medium.com/max/640/1*FZvpFJixJZ4VOKQe1Ds4Ug.gif)

### References
[(知乎) win10+wsl2+vs code+python平台搭建笔记](https://zhuanlan.zhihu.com/p/394535900)

[(知乎) WSL2: VSCode + Virtualenv的使用与配置](https://zhuanlan.zhihu.com/p/442448335)
