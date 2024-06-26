# 使用官方Ubuntu映像作為基礎
FROM ubuntu:20.04

# 設定環境變數以避免在安裝過程中出現交互式提示
ENV DEBIAN_FRONTEND=noninteractive

# 更新APT套件列表並安裝SSH服務、screen和htop
RUN apt-get update && apt-get install -y openssh-server wget screen htop libgomp1 binutils

# 設定SSH服務
RUN mkdir /var/run/sshd
RUN echo 'root:root' | chpasswd  # 請將root:root替換成更安全的密碼
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH登錄時需要的環境變數修正
RUN sed -i 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' /etc/pam.d/sshd
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# 安裝Anaconda
RUN wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh && \
    bash Anaconda3-2022.05-Linux-x86_64.sh -b && \
    rm Anaconda3-2022.05-Linux-x86_64.sh

# 將Anaconda添加到PATH
ENV PATH /root/anaconda3/bin:$PATH

# 使用conda init初始化bash shell，以便conda指令可以直接使用
RUN /root/anaconda3/bin/conda init bash

# 用conda安裝PyTorch
RUN conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia

RUN pip install sentence-transformers

RUN apt-get install -y unrar

# 清理APT緩存以減少映像大小
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 開放22端口以使用SSH
EXPOSE 22

# 啟動SSH服務
CMD ["/usr/sbin/sshd", "-D"]
