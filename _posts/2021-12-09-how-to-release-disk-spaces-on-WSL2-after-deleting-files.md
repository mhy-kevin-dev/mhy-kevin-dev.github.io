# WSL2刪除檔案仍沒釋放硬碟空間的解法
![示意圖](https://miro.medium.com/max/640/1*NYWAPnYXGsmLnmGU88dhAg.png)

### 前言
最近在公司的筆電使用WSL2開發，常常遇到C槽空間不足，即使刪除了VM裡面佔空間的資料，筆電的C槽空間仍然沒有釋放的問題。

### 解法
測試了幾個google到的方法，總算能成功清出空間，依照下列的步驟處理，應該就能釋放出空間：

1. `wsl.exe --shutdown` (記得用管理員權限打開Powershell)
2. 假設VM的版本是20.04，不同版本可以自行對照一下，進到這個路徑`cd C:\Users\<user>\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState`
3. `optimize-vhd -Path .\ext4.vhdx -Mode full`

### 改進
清空之後，建議備份wsl的ubuntu，未來要還原也比較省事

1. `cd D:\backup-vms`
2. `wsl --export Ubuntu20.04  Ubuntu20.04-2023-01.tar`

### Reference
[How do I get back unused disk space from Ubuntu on WSL2?](https://superuser.com/questions/1606213/how-do-i-get-back-unused-disk-space-from-ubuntu-on-wsl2)
