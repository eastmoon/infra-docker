# 設定 Docker 配置檔案

+ [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

## [FORM](https://docs.docker.com/engine/reference/builder/#from)

指定來源映像檔，若不存在此檔案會從 [Docker Hub](https://hub.docker.com/) 下載。

## [RUN](https://docs.docker.com/engine/reference/builder/#run)

執行腳本行為，若要執行多行可使用 Linux 多行腳本寫法，用 ```\``` 達到如下寫法

```
RUN \
    apt-get update -y && \
    apt-get install -y \
        curl
```

## [ENV](https://docs.docker.com/engine/reference/builder/#env)

設定一個用於編譯映像檔時的環境變數，例如將如下的腳本

```
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y ...
```

修改成如下腳本

```
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y ...
```

## [ARG](https://docs.docker.com/engine/reference/builder/#arg)

宣告一個變數，當使用者於編譯映像檔時，輸入並修改預設值；其宣告方式如下

```
ARG VERSION = latest
```

在編譯映像檔時 ```docker build``` 時使用參數 ```--build-arg <varname>=<value>``` 來替換目標數值

```
docekr build --build-arg VERSION=1.1.1
```
> 以此方式替換 VERSION 的數值為 1.1.1

**原則上，Dockerfile 可以透過此方式來輸入外部變數，並修改整個編譯邏輯，但於官方文件中有說明並不建議使用此方式來輸入如 ```github keys``` 等，與安全有關的數值。**

## [ADD](https://docs.docker.com/engine/reference/builder/#add) and [COPY](https://docs.docker.com/engine/reference/builder/#copy)

ADD、COPY 皆可從 HOST 主機複製檔案、目錄至指定的位置，而其差別是 ADD 可從遠端位置 ( URLs ) 複製內容。

```
ADD tmp.txt /mydir/
```

需注意，HOST 主機位置是以 Dockerfile 檔案所在位置為跟目錄，且不可移動至根目錄下一層，例如 ```ADD ../tmp.txt /mydir/```，詳細規則可參考文獻所述。

## [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose)

指定映像檔執行為容器時會偵聽的通訊埠，當 ```docker run -P``` 執行時，設定偵聽的通訊埠會自動與 HOST 開啟偵聽；但若執行 ```docker run -p``` 則可以覆蓋 ```EXPOSE``` 的設定。

## [WORKDIR](https://docs.docker.com/engine/reference/builder/#workdir)

指定映像檔執行為容器時的工作目錄，若使用 ```docker run -ti image bash``` 時，可於進入容器時來到此目錄下。

## [CMD](https://docs.docker.com/engine/reference/builder/#cmd) and [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint)

指定映像檔執行為容器時，預設會執行的腳本

```
CMD ["cmd-executable", "cmd-param1", "cmd-param2"]
ENTRYPOINT ["entry-executable", "entry-param1", "entry-param2"]
```

CMD 與 ENTRYPOINT 的差別是，CMD 會被 ```docker run -ti image [COMMAND] [ARG...]``` 的 COMMAND 替換，但若設為 ENTRYPOINT 則無法被替代，且實際執行會如下：

```
entry-executable entry-param1 entry-param2 cmd-executable cmd-param1 cmd-param2
```

由於 COMMON 可以替換後面內容，因此會執行如下：

```
entry-executable entry-param1 entry-param2 [COMMON] [ARG...]
```

因此，CMD 也被說明為 ENTRYPOINT 的參數來源，並可透過 ```docker run``` 改變參數。

## [USER](https://docs.docker.com/engine/reference/builder/#user)
