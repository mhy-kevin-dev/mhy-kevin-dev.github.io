## gridengine clustering server的錯誤解法

### ※派出的job還在跑(或是跑很久沒回應)，用qstat看主機(z840)狀態(states)是"E"

- 解決方法：重新開機，然後下指令"qmod -c all.q@ubuntu"，手動關掉"E"的狀態

> *原文：“ The "E" state is more of a concern than the 'au' state. It means that there was a major problem on  the compute node (with the system or the job itself). E states do not go away automatically, even if you reboot the  cluster. Once you think the cluster is fine you can use the "qmod"  command to clear the E state.”*

#### Reference

~~[已失效](https://arc.liv.ac.uk/pipermail/gridengine-users/2006-June/010407.html)~~

比較新的: [06 SGE administration.md](https://github.com/WGLab/biocluster/blob/master/06%20SGE%20administration.md)

### ※重啟gridengine服務後，派出的JOB仍卡在queue，用qstat看主機(dgx)狀態(states)是"au"

- 通常發生在dgx還在跑job，但整台機器被重啟

- 解決方法：重啟gridengine service

```sh
	sudo /etc/init.d/gridengine-master restart 
	sudo /etc/init.d/gridengine-exec restart
```

### ※派出的job跑很久但是沒回應，到正在run的那台機器上用htop看卻沒有job在跑 (但是qstat顯示該job是"r")，於是qdel該job又出現dr，然後卡住

- 解決方法：下指令`sudo qdel -f 該job_id`
	
> *原文：“Is there a way that my users can kill their own jobs that are stuck in the dr state?*
> 
> *qstat -f <jobid>*
> *as the user, returns job <jobid> is already in deletion*
> *yet when run as root it does get deleted*

#### Reference
  
[Kill an SGE job "already in deletion", as user](https://serverfault.com/questions/265726/kill-an-sge-job-already-in-deletion-as-user)

### ※queue status出現E，然後job都排不進去

- 解決方法：用admin賬號，下指令`qmod -c all.q`

- 接著修改可排的job數：
  
`qconf -mq all.q `
  
```
slots                 1,[dgx1-3gpu=80] 
找到這行
把80改成2
離開並儲存
```
  
- 避免機器load_avg數值過高, queue都不派job了：

`load_thresholds       np_load_avg=1.75`

- 目前遇到loading_avg 144卡住不派job,  改這樣就恢復了

`load_thresholds       np_load_avg=10`

> *原文：load_threshold*
> 
> *When threshold is exceeded, no new jobs are placed on host. Essentially it is the signal that the host is busy as determined by uptime*
> *Can use iether built-in values or values reported by custom load sensors (example: ‘logged-in-users=5’).*
> 
> *Default value*
> 
> 	np_load_avg=1.75 
>
> *lead to oversubscription of computational tasks. In many cases lower threshold such as 0.75 might be better. *
> *suspend_threshold, nsuspend, suspend_interval*
> *Similar to load_threshold but running jobs will actually be suspended/stopped. The ‘nsuspend’ param determines how many jobs per interval get suspend signals.*
> *‘suspend_interval’ defaults to 00:05:00.*
> 

#### Reference
  
http://www.softpanorama.org/HPC/Grid_engine/Queues/some_interesting_queue_attributes.shtml


### ※DGX container被重開，queue的服務沒打開，輸入qstat -f會出現下面這個錯誤訊息：

```sh
error: commlib error: got select error (Connection refused)
error: unable to send message to qmaster using port 6444 on host "940ead339e09": got send error
```
  
- 要手動打開gridengine服務，輸入指令(每輸入一次可以打qstat -f確認一下)：

```sh
sudo /etc/init.d/gridengine-master restart 
sudo /etc/init.d/gridengine-master start 
sudo /etc/init.d/gridengine-exec restart 
sudo /etc/init.d/gridengine-exec start
```

### ※gridengine排程時出現錯誤 ：

```sh
queue.pl: Error submitting jobs to queue (return status was 256)
Output of qsub was: Unable to run job: denied: host "f95869d0b506" is no submit host.
```
  
因為這個container的名字沒加到gridengine的host：
  
`qconf -as f95869d0b506 `

用這個可以看詳細的訊息：

`qstat -j <job-id>`

找error reason的欄位
