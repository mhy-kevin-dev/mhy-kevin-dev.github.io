[http://conda.pydata.org/docs/](https://docs.conda.io/en/latest/)

### 安裝 miniconda

安裝過程最後若詢問是否將 miniconda 路徑加入 .bashrc，即

`export PATH=/home/xxxxx/miniconda3/bin:$PATH`

請按 yes；不然，請在裝完後，自行加入，並且記得 source .bashrc。

```sh
cd ~
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
bash ~/miniconda.sh
conda update -n root conda
conda config --set show_channel_urls True
conda config --add channels conda-forge
```

### 建立環境

如果你想要安裝的環境是 python 2，則

`conda create -n py27 python=2.7`

如果你想要安裝的環境是 python 3，則

`conda create -n py35 python=3.5`

如果要從requirements.txt建立環境，則

`conda create --name <env> --file requirements.txt`

### 啟動環境（以 py27 為例）

`source activate py27`

### 安裝套件（以 py35 為例）

啟動環境 py35 後，你可以在此環境下 (py35) xxxxx@ubuntu 安裝需要的套件，有兩種方式：

(1) 使用 conda（以 tensorflow 為例）

`conda install tensorflow`

(2) 使用 pip（以 keras 為例）
 
`pip install keras`

- 增加更多 conda 的套件來源

```sh 
conda install anaconda
anaconda search -t conda xxx
```

- 會出現 channel、package、platform，尋找你要的版本與平台後，執行
`conda install -c xxx’s_channel xxx`

- 安裝特定版本的套件（以 numpy 為例）

```sh
conda search numpy
conda install numpy=1.11.2=py35_0
```

### 離開環境（以 py27 為例）

`source deactivate`

### 其他conda指令
https://conda.io/docs/_downloads/conda-cheatsheet.pdf


### 查看所安裝的環境

`conda info -e`

### 刪除環境（以 py27 為例）

`conda remove -n py27 —-all`

### 使用以 MKL 為基礎的環境

(1) 修改 .condarc

`vim ~/.condarc`

(2) 將 - conda-forge 註解掉
`# - conda-forge`

(3) 更新 conda

`conda update conda`

(4) 進入 python 3.5 環境

`conda install numpy scipy scikit-learn pandas mkl-service`

(5) 安裝 pip 套件
 
```sh
pip install bob.measure bob.learn.em
pip install pydot-ng
sudo apt install graphviz
conda install h5py
pip install keras
```

(6) 設定 mkl 核心數

```py
import mkl
mkl.get_max_threads()
mkl.set_num_threads(x)
```

