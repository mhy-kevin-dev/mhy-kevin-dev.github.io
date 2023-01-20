### 安裝MITLM

```sh
sudo -I
cd /usr/local
git clone https://github.com/mitlm/mitlm
apt-get install autoconf   ##(>=2.69)
apt-get install automake
apt-get install libtool
apt-get install autoconf-archive
sudo ./autogen.sh
sudo ./configure
sudo make
sudo make install
```

預設會安裝在 /usr/local/lib 

接著在 path.sh 或 ~/.bashrc 加上環境變數，就可以直接使用mitlm的指令了

`export LD_LIBRARY_PATH=${LD_LIBRARY_PATH:-}:${LIBLBFGS}/lib/.libs:/usr/local/lib`

之後可以直接打 estimate-ngram 來使用


### next post

這篇的內容會在這使用： [Baseline-Language-Model-Training](https://mhy-kevin-dev.github.io)
