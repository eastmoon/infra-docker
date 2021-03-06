# Docker 基礎操作

## 操作

Docker啟動容器(Container)的方式是先檢查本地端的image，若不存在本地的image則會到docker hub搜尋；因此，若要建立官方提供的基底，可藉由docker hub上先行確認後下載。

### 映像檔
```
> sudo docker pull Image-name:Image-version
```
> 依據映像名稱(Image-name)、版本(Image-version)，從docker hub上取得映像檔案。

### 執行

```
> sudo docker run -t -i Image-name:Image-version
```

Docker 依據不同的執行參數，會讓容器處在背景 ( Detached ) 或前台 ( Foregroud ) 模式；背景意旨啟動的容器將只能透過網路或共享來互動，不可使用命令模式直接控制；以下列舉常用參數，詳細參數說明參考 [Docker run reference](https://docs.docker.com/engine/reference/run/)

+ ```-t, --tty```：開啟終端模式(tty，Teletype，終端設備統稱)進入容器
+ ```-i```：透過標準輸入(stdin)與容器溝通
+ ```-a```：指定標準輸入(stdin)、輸出(stdout)、錯誤(stderr)與容器溝通；默認上執行全部，但可透過此做獨力掛載
+ ```-d```：背景執行容器
+ ```--rm```：容器終止操作時時自動移除容器
+ ```--entrypoint```：由輸入內容替換 Dockerfile 中 ENTRYPOINT 設定內容
    - 多數容器啟動常見指定 ```/bin/bash```、```docker-entrypoint.sh```，前者是 CMD、ENTRYPOINT 皆未設定時預設執行的內容，後者多是容器編譯時透過 ADD 複製入映像檔的啟動程序
    - 若要除錯容器狀態，且要迴避 CMD、ENTRYPOINT 則可以用 ```docker run -ti --entrypoint bash <image>``` 來解除原始設定並進入容器內操作；需注意，能否使用 ```bash``` 或需替換為 ```/bin/bash```、```sh```、```/bin/sh``` 是根據映像檔的根源作業系統決定。

### 管理

+ 顯示當前執行中的容器
```
> sudo docker ps
```

+ 顯示所有容器
```
> sudo docker ps -a
```

+ 顯示'Exited'(退出)的容器
```
> sudo docker ps -a | grep Exited
```

+ 移除特定容器
```
> sudo docker rm [container-name]
```

+ 移除所有容器；執行中的容器，不會被移除。
```
> sudo docker ps -a -q | xargs sudo docker rm
```

+ 移除所有容器；保括執行中的容器。
```
> sudo docker ps -a -q | xargs sudo docker rm -f
```

+ 移除'Exited'(退出)的容器
```
> sudo docker ps -a | grep Exited | cut -d ' ' -f 1 | xargs sudo docker rm
```
> 連續執行命令，先透過docker取得所有執行容器，在透過grep過濾退出容器，在經由cut命令逐行依空白分割後保留第一行，在由xargs將輸出的字串陣列當成參數傳入docker rm指令。

### 封裝

透過Docker run執行後，將有過改變的映像檔封裝成新檔案，以利日後運用。
```
> sudo docker commit -m "infomation" -a "Docker author" [Container] [Respository]
```

範例：
```
sudo docker commit -m "Install traceroute" -a "Docker newbie" 3c94df088985 ubuntu-update:1
```

### 建製

透過Docker build將Dockerfile當做建立檔案，逐步將映像檔建立完成。
```
> sudo docker build --rm -t 'image-name:image-version' .
```

> --rm，刪除無用的安裝檔案
> '.'，指定此檔案夾中的Dockerfile為檔案。

## 文獻

+ [Docker 實作入門] (https://www.openfoundry.org/tw/tech-column/9319-docker-101)
+ [Docker —— 從入門到實踐，取得映像檔] (https://philipzheng.gitbooks.io/docker_practice/content/image/)
+ [Docker run 命令的使用方法] (http://dockone.io/article/152)
+ [比較 save, export 對於映象檔操作差異] (https://blog.hinablue.me/docker-bi-jiao-save-export-dui-yu-ying-xiang-dang-cao-zuo-chai-yi/)
+ [背景執行] (http://docker.readbook.tw/docker/basic/102-container/daemon/index.html)
+ [Docker Network] (http://dockone.io/article/393)
