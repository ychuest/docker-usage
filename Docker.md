# 使用docker技术启动打包好的vllm镜像
### docker简要介绍：
1. docker daemon：docker守护进程，用来侦听docker API请求和管理docker对象，例如镜像，容器等。
2. docker registries：存储docker镜像的仓库，比如docker hub。可以通过以下命令配置docker hub镜像代理。
   ~~~
   # 创建配置文件 /etc/systemd/system/docker.service.d/http-proxy.conf 
   # 写入以下配置内容
   [Service]
   Environment=http_proxy=http://proxy.lan:11081 
   https_proxy=http://proxy.lan:11081
   
   
   # 加载配置
   systemctl daemon-reload
   # 重启docker
   systemctl restart docker
   ~~~
3. docker objects：
   + images：镜像，是用于创建docker容器的只读模板，可以通过Dockerfile来构建定制的images。
   + containers：容器，是镜像的可运行实例。

### docker启动镜像主要步骤：

1. **安装docker：** 以Ubuntu系统为例，下面有三个教程可以参考。
   - [How To Install Docker on Ubuntu 24.04/22.04/20.04 | ComputingForGeeks](https://computingforgeeks.com/how-to-install-docker-on-ubuntu/)
   - [在 Ubuntu Server 22.04 上安装 Docker 的详细步骤_ubuntu22.04安装docker-CSDN博客](https://blog.csdn.net/dw14132124/article/details/140510584?spm=1001.2014.3001.5501)
   - [Install Docker Engine on Ubuntu | Docker Docs](https://docs.docker.com/engine/install/ubuntu/)（官方文档）

    基本上都是更新软件包，安装基本依赖，删除旧版本docker，导入gpg秘钥，添加docker ce（docker社区版）仓库，安装docker ce，最后启动docker服务。

    ~~~
    # 删除旧版本docker及其依赖
    sudo apt remove docker docker-engine docker.io containerd runc

    # 更新系统软件包
    sudo apt -y update
    
    # 安装docker依赖
    sudo apt -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
    
    # 添加docker中科大GPG密钥
    sudo mkdir -p /etc/apt/keyrings
    sudo curl -fsSL http://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc
    
    # 添加 Docker 中科大镜像稳定版软件源
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] http://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    
    # 安装docker ce
    sudo apt update
    sudo apt install docker-ce docker-ce-cli containerd.io

    # 启用docker服务
    sudo systemctl start docker

    # 添加用户账户到docker组（可选，若未添加用户，则执行docker命令时要加sudo）
    sudo usermod -aG docker $USER
    newgrp docker
    ~~~

    **注意：**
    -	使用官方源可能会添加失败，可以更换镜像源，比如中科大镜像源http://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu，清华镜像源https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu。
    -	安装docker之前可以替换apt源为清华源：[ubuntu | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)，记得选择对应的Ubuntu版本，这里以Ubuntu 22.04 为例。
        ~~~
        sudo nano /etc/apt/sources.list    # ubuntu 24.04 版本之前为该目录
        # 替换内容为：
        
        # 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
        deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
        # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
        deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
        # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
        deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
        # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

        # 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
        deb http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
        # deb-src http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse

        # 预发布软件源，不建议启用
        # deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
        # # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
        ~~~
 
2. **安装NVIDIA Container Toolkit：** 以Ubuntu系统为例，下面有两个教程可以参考。
   - [Installing the NVIDIA Container Toolkit — NVIDIA Container Toolkit 1.16.0 documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)（Nvidia 官方文档）
   - [NVIDIA Container Toolkit 安装与配置帮助文档(Ubuntu,Docker)-CSDN博客](https://blog.csdn.net/dw14132124/article/details/140534628?spm=1001.2014.3001.5501)

    主要步骤为配置仓库、更新软件包列表、安装 NVIDIA Container Toolkit 软件包。
    ~~~
    # 配置仓库
    curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
    && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

    # 更新软件包列表
    sudo apt-get update

    # 安装 NVIDIA Container Toolkit 软件包
    sudo apt-get install -y nvidia-container-toolkit

    # 配置容器运行时（也可以通过修改daemon.json文件来进行配置，如步骤3）
    sudo nvidia-ctk runtime configure --runtime=docker

    # 重新启动docker守护进程
    sudo systemctl restart docker
    ~~~
3. **修改/etc/docker/daemon.json：**
    ~~~
    sudo nano /etc/docker/daemon.json
    
    # 把/etc/docker/daemon.json中内容替换为下面的配置：
    {
        "bip": "172.17.0.1/16",
        "default-address-pools": [
            {
                "base": "172.18.0.0/16",
                "size": 26
            }
        ],
        "ipv6": false,
        "runtimes": {
            "nvidia": {
                "args": [],
                "path": "nvidia-container-runtime"
            }
        }
    }
    

    # 加载配置
    sudo systemctl daemon-reload
    # 重启docker
    sudo systemctl restart docker
    ~~~


4.	**加载镜像：**
    ~~~
    # 创建一个目录用来放配置文件和模版：
    mkdir vllm-docker
    # 将docker-compose-qwen2-72b-tool-use.yaml和tool_chat_template_qwen.jinja放到vllm-docker目录下。
    # 查看docker状态：
    sudo systemctl status docker
    # 启动docker：
    sudo systemctl start docker
    # 转到有镜像的目录下：
    cd existingfolder
    # 加载镜像：
    sudo docker load -i vllm-qwen-usetool.tar
    ~~~
    [docker-compose.yml 参数详解 - ABEELAN - 博客园 (cnblogs.com)](https://www.cnblogs.com/abeelan/p/17251208.html)


5.	**启动vllm服务：**
    ~~~
    sudo docker compose up      # 按照docker-compose.yml中的配置启动服务
    ~~~