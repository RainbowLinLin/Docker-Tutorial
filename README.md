# Docker-Tutorial

***Image***

映像檔，可以把它想成是以前我們在玩 VM 的 Guest OS（ 安裝在虛擬機上的作業系統 ）。

Image 是唯讀（ R\O ）

***Container***

容器，利用映像檔（ Image ）所創造出來的，一個 Image 可以創造出多個不同的 Container，

Container 也可以被啟動、開始、停止、刪除，並且互相分離。

## 指令介紹

接著介紹一些 Docker 的指令，

Docker 的指令真的很多，這裡就介紹我比較常用的或是實用的指令

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
