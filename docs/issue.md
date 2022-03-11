# 技術議題

## [Nvidia-Docker](https://github.com/NVIDIA/nvidia-docker)

讓 Docker 運行直接運作於 GPU 之上而非 CPU。

+ [A Primer on Nvidia-Docker — Where Containers Meet GPUs](https://thenewstack.io/primer-nvidia-docker-containers-meet-gpus/)

## [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

在 Windows 上運作 Linux 環境，在早期版本中可用方式遠低於 Docker；但 Docker 目前已可運用 WSL2 來進行虛擬容器運作。

+ [Windows Subsystem for Linux 環境配置](https://medium.com/hungys-blog/windows-subsystem-for-linux-configuration-caf2f47d0dfb)

## Service、Systemctl

在 Docker 安裝環境時，多數服務會碰比需啟動運作程序的問題，例如 MySQL、MongoDB 需要開啟資料庫服務，簡單的方式可以將啟動程序寫在 .bashrc 以便運作，但如何啟用服務，以及如何分離使用者執行服務，避免資安爭議則是需要研究的議題。

+ [鳥哥的 Linux 私房菜 - 開機關機流程與 Loader](http://linux.vbird.org/linux_basic/0510osloader/0510osloader-fc4.php)
+ [鳥哥的 Linux 私房菜 - 認識系統服務 (daemons)](http://linux.vbird.org/linux_basic/0560daemons.php)
+ [Start service using systemctl inside docker container](https://stackoverflow.com/questions/46800594)
+ [Docker How to run /usr/sbin/init and then other scripts on start up](https://stackoverflow.com/questions/48720049)
+ [為何在 Docker 中執行特權容器不是個好主意?](https://blog.trendmicro.com.tw/?p=62986)
+ [sudo or gosu](http://programmersought.com/article/82831278699/)

在 Docker Hub 上可以看到大多官方的 Docker 容器都有運用 gosu 的方式，避免容器產生資安疑慮；在自建容器時可以因精簡設計而先規避使用，但進入產品環境實則需確實使用以避免產生資安漏洞。

## [Docker 無法連線至特定網段 (172.17.x.x)](https://blog.yowko.com/docker-172-17-ip/)

具有一定規模公司或採用虛擬環境的架構，常用網段不外乎 192.x.X.X 或 172.x.x.x，這使得在這些網段中使用 Docker 會導致路由錯誤，導致無法連線到正確的主機；因此，若碰到此狀況則需調整 Docker 的啟動網段設定。

需注意兩個問題：

1. [Daemon](https://docs.docker.com/config/daemon/)的相關 host 設定在 Docker Desktop for Windows 或 Mac 會無法正常運作
2. 多數文獻提到 ```daemon.json``` 檔案在 ```C:\ProgramData\Docker\config```，但經測試後發現 Windows 10 的環境是依據用戶帳戶下的設定檔來運作 ```C:/Users/<Username>/.docker/```；日後是否會在改變還需參考 Docker 實際變更資訊或資料結構來實測
