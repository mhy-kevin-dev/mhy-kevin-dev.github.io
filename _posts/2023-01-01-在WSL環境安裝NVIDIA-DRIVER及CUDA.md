
### 不要在WSL內直接下指令安裝nvidia driver

1. 在windows安裝driver, https://www.nvidia.com/Download/index.aspx 選擇對應的版本
2. 安裝完，在windows的cmd下`nvidia-smi`，如果正常顯示GPU狀態，表示安裝完成
3. 到 https://developer.nvidia.com/cuda-downloads  查詢wsl2用的安裝指令, 記得版本要選Ubuntu-wsl的
4. 開啟WSL，在裏面貼步驟三的指令一一安裝

![步驟3-圖1](https://i.imgur.com/dn5BQNi.png)
![步驟3-圖2](https://i.imgur.com/3RBN3s1.png)

### reference
https://www.bilibili.com/read/cv14608547
