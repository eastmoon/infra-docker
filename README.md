# Docker

Vagrant 是基於 Infrastructure as Code (IaC) 概念的虛擬環境操作工具，其操作裝置可以是 VirtualBox、VMWave 等。

+ [Docker](https://www.docker.com/)

## 操作

操作範例使用 Vagrant + VirtualBox 為範本

+ [基礎操作](./docs/operate.md)
+ 安裝
    - [Docker for Windows](./docs/docker-for-windows.md)
    - [Docker for Linux](./docs/docker-for-windows.md)
    - [Docker run GPU on Windows](./docs/docker-run-GPU-on-windows.md)
+ [設定 Dockerfile](./docs/configure-dockerfile.md)
+ [設定 docker-compose](./docs/configure-dockerfile.md)
+ [技術議題](./docs/issue.md)

## 實務範例參考

範例以實務中常用工具或管理設定為範本；其他軟體用運則參考相關專案內的 Docker 設定

+ [git](./Dockerfile/git)
+ [curl](./Dockerfile/curl)
+ [sshpass](./Dockerfile/sshpass)
