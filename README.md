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
* `--gpus all` 使用全部的GPU   
* `--name` 設定 container 的名稱   
* `-i` 讓容器的標準輸入保持打開  
* `-t` 讓Docker分配一個虛擬終端（pseudo-tty）並綁定到容器的標準輸入上  
* `-it` -i -t 的合併寫法, 搞不懂這2個在幹嘛, 打上去就對了  
* `--shm-size 16G` 設定container中/dev/shm 的大小為16G, 沒有設定的話預設為64M, 此大小在pytorch中會有error  
* `-v /home/r07127:/r07127` 把Velkoz中/home/r07127的資料夾link到Container中的/r07127資料夾  
* `pytorch/pytorch:1.1.0-cuda10.0-cudnn7.5-devel` 要用來啟動container的Image檔  
* `bash` 你要執行的指令, 可以先打bash, 進入container後在下其他指令    
  
輸入`pip list`確定pytorch版本為1.1.0
```cmd
root@336dc392dea0:/workspace# pip list
Package          Version
---------------- --------
asn1crypto       0.24.0
backcall         0.1.0
beautifulsoup4   4.7.1
certifi          2019.3.9
cffi             1.12.3
chardet          3.0.4
conda            4.6.14
conda-build      3.17.8
cryptography     2.6.1
decorator        4.4.0
filelock         3.0.10
glob2            0.6
idna             2.8
ipython          7.5.0
ipython-genutils 0.2.0
jedi             0.13.3
Jinja2           2.10.1
libarchive-c     2.8
lief             0.9.0
MarkupSafe       1.1.1
mkl-fft          1.0.12
mkl-random       1.0.2
numpy            1.16.3
olefile          0.46
parso            0.4.0
pexpect          4.7.0
pickleshare      0.7.5
Pillow           6.0.0
pip              19.1
pkginfo          1.5.0.1
prompt-toolkit   2.0.9
psutil           5.6.2
ptyprocess       0.6.0
pycosat          0.6.3
pycparser        2.19
Pygments         2.3.1
pyOpenSSL        19.0.0
PySocks          1.6.8
pytz             2019.1
PyYAML           5.1
requests         2.21.0
ruamel-yaml      0.15.46
setuptools       41.0.1
six              1.12.0
soupsieve        1.8
torch            1.1.0
torchvision      0.2.2
tqdm             4.31.1
traitlets        4.3.2
urllib3          1.24.2
wcwidth          0.1.7
wheel            0.33.1
```
輸入`nvcc --version`確認cuda版本為10.0
```cmd
root@336dc392dea0:/workspace# nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130
```
確認以上版本都是自己需要的以後  
移動到自己link進container的資料夾, 並且執行自己的程式(此處以pytorch_mnist.py為例)
```cmd
root@336dc392dea0:/workspace# cd ../r07127/Docker-Tutorial/
root@336dc392dea0:/r07127/Docker-Tutorial# python pytorch_mnist.py
Downloading http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz to ./MNIST/MNIST/raw/train-images-idx3-ubyte.gz
 97%|#########################################################################################################1  | 9650176/9912422 [00:12<00:00, 2049268.76it/s]Extracting ./MNIST/MNIST/raw/train-images-idx3-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz to ./MNIST/MNIST/raw/train-labels-idx1-ubyte.gz
                                                                                                                                                               Extracting ./MNIST/MNIST/raw/train-labels-idx1-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz to ./MNIST/MNIST/raw/t10k-images-idx3-ubyte.gz
                                                                                                                                                               Extracting ./MNIST/MNIST/raw/t10k-images-idx3-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz to ./MNIST/MNIST/raw/t10k-labels-idx1-ubyte.gz
                                                                                                                                                               Extracting ./MNIST/MNIST/raw/t10k-labels-idx1-ubyte.gz
Processing...
Done!
Train Epoch: 1 [0/60000 (0%)]   Loss: 2.300039
Train Epoch: 1 [640/60000 (1%)] Loss: 2.213450
Train Epoch: 1 [1280/60000 (2%)]        Loss: 2.170429
Train Epoch: 1 [1920/60000 (3%)]        Loss: 2.076518
Train Epoch: 1 [2560/60000 (4%)]        Loss: 1.868044
Train Epoch: 1 [3200/60000 (5%)]        Loss: 1.413765
Train Epoch: 1 [3840/60000 (6%)]        Loss: 1.000020
Train Epoch: 1 [4480/60000 (7%)]        Loss: 0.775462
Train Epoch: 1 [5120/60000 (9%)]        Loss: 0.460300
Train Epoch: 1 [5760/60000 (10%)]       Loss: 0.486124
Train Epoch: 1 [6400/60000 (11%)]       Loss: 0.437753
Train Epoch: 1 [7040/60000 (12%)]       Loss: 0.408149
Train Epoch: 1 [7680/60000 (13%)]       Loss: 0.458510
Train Epoch: 1 [8320/60000 (14%)]       Loss: 0.427587
Train Epoch: 1 [8960/60000 (15%)]       Loss: 0.398676
Train Epoch: 1 [9600/60000 (16%)]       Loss: 0.386188
Train Epoch: 1 [10240/60000 (17%)]      Loss: 0.298156
Train Epoch: 1 [10880/60000 (18%)]      Loss: 0.502031
Train Epoch: 1 [11520/60000 (19%)]      Loss: 0.523246
Train Epoch: 1 [12160/60000 (20%)]      Loss: 0.339011
Train Epoch: 1 [12800/60000 (21%)]      Loss: 0.367665
Train Epoch: 1 [13440/60000 (22%)]      Loss: 0.451440
Train Epoch: 1 [14080/60000 (23%)]      Loss: 0.304708
Train Epoch: 1 [14720/60000 (25%)]      Loss: 0.358396
Train Epoch: 1 [15360/60000 (26%)]      Loss: 0.331111
Train Epoch: 1 [16000/60000 (27%)]      Loss: 0.440046
Train Epoch: 1 [16640/60000 (28%)]      Loss: 0.363632
Train Epoch: 1 [17280/60000 (29%)]      Loss: 0.314917
Train Epoch: 1 [17920/60000 (30%)]      Loss: 0.200644
Train Epoch: 1 [18560/60000 (31%)]      Loss: 0.500168
Train Epoch: 1 [19200/60000 (32%)]      Loss: 0.326233
Train Epoch: 1 [19840/60000 (33%)]      Loss: 0.120071
Train Epoch: 1 [20480/60000 (34%)]      Loss: 0.189754
Train Epoch: 1 [21120/60000 (35%)]      Loss: 0.139856
Train Epoch: 1 [21760/60000 (36%)]      Loss: 0.312687
Train Epoch: 1 [22400/60000 (37%)]      Loss: 0.150788
Train Epoch: 1 [23040/60000 (38%)]      Loss: 0.288335
Train Epoch: 1 [23680/60000 (39%)]      Loss: 0.467620
9920512it [00:29, 2049268.76it/s]                                                                                                                               Train Epoch: 1 [24320/60000 (41%)]      Loss: 0.215443
Train Epoch: 1 [24960/60000 (42%)]      Loss: 0.153275
Train Epoch: 1 [25600/60000 (43%)]      Loss: 0.224636
Train Epoch: 1 [26240/60000 (44%)]      Loss: 0.263891
Train Epoch: 1 [26880/60000 (45%)]      Loss: 0.233028
Train Epoch: 1 [27520/60000 (46%)]      Loss: 0.264711
Train Epoch: 1 [28160/60000 (47%)]      Loss: 0.210201
Train Epoch: 1 [28800/60000 (48%)]      Loss: 0.132909
Train Epoch: 1 [29440/60000 (49%)]      Loss: 0.277988
Train Epoch: 1 [30080/60000 (50%)]      Loss: 0.094118
Train Epoch: 1 [30720/60000 (51%)]      Loss: 0.128409
Train Epoch: 1 [31360/60000 (52%)]      Loss: 0.246995
Train Epoch: 1 [32000/60000 (53%)]      Loss: 0.336425
Train Epoch: 1 [32640/60000 (54%)]      Loss: 0.153679
Train Epoch: 1 [33280/60000 (55%)]      Loss: 0.091901
Train Epoch: 1 [33920/60000 (57%)]      Loss: 0.145015
Train Epoch: 1 [34560/60000 (58%)]      Loss: 0.197816
Train Epoch: 1 [35200/60000 (59%)]      Loss: 0.218493
Train Epoch: 1 [35840/60000 (60%)]      Loss: 0.062733
Train Epoch: 1 [36480/60000 (61%)]      Loss: 0.135493
Train Epoch: 1 [37120/60000 (62%)]      Loss: 0.116680
Train Epoch: 1 [37760/60000 (63%)]      Loss: 0.236761
Train Epoch: 1 [38400/60000 (64%)]      Loss: 0.063430
Train Epoch: 1 [39040/60000 (65%)]      Loss: 0.106552
Train Epoch: 1 [39680/60000 (66%)]      Loss: 0.160519
Train Epoch: 1 [40320/60000 (67%)]      Loss: 0.107377
Train Epoch: 1 [40960/60000 (68%)]      Loss: 0.177162
Train Epoch: 1 [41600/60000 (69%)]      Loss: 0.229493
Train Epoch: 1 [42240/60000 (70%)]      Loss: 0.073655
Train Epoch: 1 [42880/60000 (71%)]      Loss: 0.153484
Train Epoch: 1 [43520/60000 (72%)]      Loss: 0.277725
Train Epoch: 1 [44160/60000 (74%)]      Loss: 0.142475
Train Epoch: 1 [44800/60000 (75%)]      Loss: 0.115020
Train Epoch: 1 [45440/60000 (76%)]      Loss: 0.120996
Train Epoch: 1 [46080/60000 (77%)]      Loss: 0.077347
Train Epoch: 1 [46720/60000 (78%)]      Loss: 0.194605
Train Epoch: 1 [47360/60000 (79%)]      Loss: 0.069361
Train Epoch: 1 [48000/60000 (80%)]      Loss: 0.211329
Train Epoch: 1 [48640/60000 (81%)]      Loss: 0.138081
Train Epoch: 1 [49280/60000 (82%)]      Loss: 0.093662
Train Epoch: 1 [49920/60000 (83%)]      Loss: 0.107775
Train Epoch: 1 [50560/60000 (84%)]      Loss: 0.120280
Train Epoch: 1 [51200/60000 (85%)]      Loss: 0.146696
Train Epoch: 1 [51840/60000 (86%)]      Loss: 0.068481
Train Epoch: 1 [52480/60000 (87%)]      Loss: 0.024163
Train Epoch: 1 [53120/60000 (88%)]      Loss: 0.260879
Train Epoch: 1 [53760/60000 (90%)]      Loss: 0.092017
Train Epoch: 1 [54400/60000 (91%)]      Loss: 0.129662
Train Epoch: 1 [55040/60000 (92%)]      Loss: 0.189733
Train Epoch: 1 [55680/60000 (93%)]      Loss: 0.034060
Train Epoch: 1 [56320/60000 (94%)]      Loss: 0.035735
Train Epoch: 1 [56960/60000 (95%)]      Loss: 0.078727
Train Epoch: 1 [57600/60000 (96%)]      Loss: 0.115442
Train Epoch: 1 [58240/60000 (97%)]      Loss: 0.192014
Train Epoch: 1 [58880/60000 (98%)]      Loss: 0.206907
Train Epoch: 1 [59520/60000 (99%)]      Loss: 0.063385

Test set: Average loss: 0.1013, Accuracy: 9668/10000 (97%)
...(略)
```
在執行pytorch_mnist.py的同時可以觀察2顆GPU是否正常運作
```cmd
r07127@Velkoz:~$ nvidia-smi -l 1
Fri Aug  2 01:23:56 2019
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 415.27       Driver Version: 415.27       CUDA Version: 10.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 108...  Off  | 00000000:01:00.0 Off |                  N/A |
|  0%   54C    P2    67W / 275W |    581MiB / 11177MiB |     14%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce GTX 108...  Off  | 00000000:02:00.0 Off |                  N/A |
|  0%   45C    P2    62W / 275W |    565MiB / 11178MiB |      9%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1296      G   /usr/lib/xorg/Xorg                             9MiB |
|    0      1342      G   /usr/bin/gnome-shell                           6MiB |
|    0     23543      C   python                                       553MiB |
|    1     23543      C   python                                       553MiB |
+-----------------------------------------------------------------------------+
```
