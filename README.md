# Docker-Tutorial

***Image***

映像檔，可以把它想成是以前我們在玩 VM 的 Guest OS（ 安裝在虛擬機上的作業系統 ）。

Image 是唯讀（ R\O ）

***Container***

容器，利用映像檔（ Image ）所創造出來的，一個 Image 可以創造出多個不同的 Container，

Container 也可以被啟動、開始、停止、刪除，並且互相分離。

## 指令介紹

查看目前 images
```cmd
docker images ls
```
刪除 Image
```cmd
docker rmi [OPTIONS] IMAGE [IMAGE...]
```
查看目前運行的 container
```cmd
docker ps
```
查看目前全部的 container（ 包含停止狀態的 container ）
```cmd
docker ps -a
```
新建並啟動 Container
```cmd
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```
啟動 Container
```cmd
docker start [OPTIONS] CONTAINER [CONTAINER...]
```
停止 Container
```cmd
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```
重新啟動 Container
```cmd
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```
删除 Container
```cmd
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```
進入 Container
```cmd
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
docker exec -it <Container ID> bash
```
## 範例(以Velkoz為例)
查看目前 images  
可以看到目前已有許多不同版本的Pytorch以及Tensorflow可供使用
```cmd
r07127@Velkoz:~$ docker image ls
REPOSITORY                 TAG                             IMAGE ID            CREATED             SIZE
nvidia/cuda                10.0-cudnn7-devel-ubuntu16.04   edbd625b9042        2 days ago          3.11GB
nvidia/cuda                10.0-cudnn7-devel-ubuntu18.04   bdc0497c2295        2 days ago          3.07GB
ubuntu                     latest                          3556258649b2        9 days ago          64.2MB
tensorflow/tensorflow      1.13.2-gpu-py3                  b5408c35298a        2 weeks ago         3.35GB
tensorflow/tensorflow      1.14.0-gpu-py3                  a7a1861d2150        5 weeks ago         3.51GB
pytorch/pytorch            1.1.0-cuda10.0-cudnn7.5-devel   1be771bff6d1        3 months ago        6.94GB
pytorch/pytorch            1.0.1-cuda10.0-cudnn7-devel     1d67d1a473d2        5 months ago        6.17GB
pytorch/pytorch            1.0-cuda10.0-cudnn7-devel       fa5b91571a44        7 months ago        5.45GB
tensorflow/tensorflow      1.12.0-gpu-py3                  413b9533f92a        8 months ago        3.35GB
tensorflow/tensorflow      1.11.0-gpu-py3                  888a203f096f        10 months ago       3.27GB
tensorflow/tensorflow      1.10.0-gpu-py3                  0b4ceed1758b        11 months ago       3.08GB
pytorch/pytorch            0.4.1-cuda9-cudnn7-devel        3febd5b72fb0        12 months ago       5.23GB
pytorch/pytorch            0.4-cuda9-cudnn7-devel          63994d8624a2        13 months ago       4.71GB
```
查看目前全部的 container  
可以發現目前沒有任何被建立的container
```cmd
r07127@Velkoz:~$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
新建並啟動 Container  
以pytorch 1.1.0-cuda10.0-cudnn7.5-devel為範例
```cmd
r07127@Velkoz:~$ docker run --gpus all --name=r07127_pytorch_1.1 -it --shm-size 16G -v /home/r07127:/r07127 pytorch/pytorch:1.1.0-cuda10.0-cudnn7.5-devel bash
```
`--gpus all` 使用全部的GPU   
`--name` 設定 container 的名稱   
`-i` 讓容器的標準輸入保持打開  
`-t` 讓Docker分配一個虛擬終端（pseudo-tty）並綁定到容器的標準輸入上  
`-it` -i -t 的合併寫法, 搞不懂這2個在幹嘛, 打上去就對了  
`--shm-size 16G` 設定container中/dev/shm 的大小為16G, 沒有設定的話預設為64M, 此大小在pytorch中會有error  
`-v /home/r07127:/r07127` 把Velkoz中/home/r07127的資料夾link到Container中的/r07127資料夾  
`pytorch/pytorch:1.1.0-cuda10.0-cudnn7.5-devel` 要用來啟動container的Image檔  
`bash` 你要執行的指令, 可以先打bash, 進入container後在下其他指令  

