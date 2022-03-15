# Docker 安裝

## 簡介

![Nvidia Docker architecture](./img/nvidia-docker.png)

Docker 一種是服務虛擬機，其特徵是將作業系統核心與服務分離，讓核心以軟體安裝於目標主機，在載入服務虛擬機；而這樣的機制若要將運算核心從 CPU 替換為 GPU 就必需從軟體本身替換與設定，才有辦法讓其服務虛擬機運用 GPU 的運算指令。

由深度學習 ( Deep Learning ) 的矩陣運算仰賴需 GPU 服務來加速效率，雖可運用 CPU 替代，但多數軟體會直接綁定 GPU 服務，這導致軟體運用的限制：

+ Linux Operating System
+ Nvidia GPU driver

而符合此規格的設備，多為伺服器、算圖主機等，非開發人員、一般用戶的主流電腦設備。

因此，在 Microsoft、Nvidia、Docker 三方的合作下，提出了 Nvidia-Docker；然而初期的 Nvidia-Docker 仍限制於 Linux 環境 ( 演進史詳閱 [Is GPU pass-through possible with docker for Windows?](https://stackoverflow.com/questions/49589229) )，至今則可透過 Windows Subsystem for Linux 安裝 Docker 版本並匯入 Nvidia Container Toolskit 來達到讓 Windows 環境的 Docker 可利用 GPU 運行。

然而，此項服務執行仍有一下限制 ( 2021.08 )
> [Windows and WSL version checking](https://www.bleepingcomputer.com/news/microsoft/windows-10-tests-wsl2-linux-kernel-updates-via-windows-update/)

+ NVIDIA Driver for Windows 10 and Windows 11
    - 在控制台中使用 ```cmd.exe /c "systeminfo"``` 確認
+ WIP Build: 22000.160 on devchannel, 19044.1200 on release preview channel
    - 在控制台中使用 ```cmd.exe /c "ver"``` 確認
+ WSL Linux Kernel 5.10.43
    - 進入 WSL 服務後使用 ```uname -a``` 確認

**至 2022.03 為止，Windows 用戶環境仍無法將 WSL 版本提升至需要版本，依據文件必須加入開發社群或自行編譯 [WSL2 Linux Kernel](https://github.com/microsoft/WSL2-Linux-Kernel)**

## 安裝
> 由於個人主機於最後步驟仍因缺乏必要套件而無法正常執行，在此僅做記錄，待日後確認實務狀況

+ 建立工作目錄
```
mkdir nvidia-docker
cd nvidia-docker
```

+ 建立 wsl 2 容器
```
wsl --install -d Ubuntu-20.04
wsl --export Ubuntu-20.04 nvidia-docker.tar
wsl --import nvidia-docker . nvidia-docker.tar
wsl -d nvidia-docker
```

+ 安裝 Docker
```
curl https://get.docker.com | sh    
```

+ 安裝 NVIDIA Container Toolkit
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$(. /etc/os-release;echo $ID$VERSION_ID)/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update -y && sudo apt-get install -y nvidia-docker2
sudo service docker stop && sudo service docker start
```

## 文獻

+ [CUDA on Windows Subsystem for Linux (WSL)](https://developer.nvidia.com/cuda/wsl)
    - [CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)
+ [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker/blob/master/README.md)
    - [nvidia/cuda - Docker](https://hub.docker.com/r/nvidia/cuda)
+ [Enable NVIDIA CUDA on WSL](https://docs.microsoft.com/zh-tw/windows/ai/directml/gpu-cuda-in-wsl)
+ [WSL 2 GPU Support for Docker Desktop on NVIDIA GPUs](https://www.docker.com/blog/wsl-2-gpu-support-for-docker-desktop-on-nvidia-gpus/)
    - [Windows 容器中的 GPU 加速](https://docs.microsoft.com/zh-tw/virtualization/windowscontainers/deploy-containers/gpu-acceleration)，依據說明此方式並不適用 Docker Desktop 啟用 Hyper-v 的情況。
+ [WSL](https://docs.microsoft.com/zh-tw/windows/wsl/)
    - [WSL 的基本命令](https://docs.microsoft.com/zh-tw/windows/wsl/basic-commands)
    - [匯入任何要與 WSL 搭配使用的 Linux 散發套件](https://docs.microsoft.com/zh-tw/windows/wsl/use-custom-distro)
+ ISSUE
    - [Unable to start container with GPU, docker: Error response from daemon: OCI runtime create failed.](https://github.com/NVIDIA/nvidia-docker/issues/1441)
    - [Is GPU pass-through possible with docker for Windows?](https://stackoverflow.com/questions/49589229)
