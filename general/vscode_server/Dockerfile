# 使用 codercom/code-server 的最新版作為基礎映像
FROM codercom/code-server:latest

# 設定容器內的使用者為 root
USER root

# 設置環境變數
ENV PASSWORD=password \
    SUDO_PASSWORD=password

# 設定環境變數以避免在安裝過程中出現交互式提示
ENV DEBIAN_FRONTEND=noninteractive

# 更新APT套件列表並安裝SSH服務、screen和htop
RUN apt-get update && apt-get install -y openssh-server wget screen htop

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

# 清理APT緩存以減少映像大小
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 設定工作目錄
WORKDIR /home/coder/project

# 暴露端口
EXPOSE 8080

# 開放22端口以使用SSH
EXPOSE 22

# 啟動SSH服務
CMD ["/usr/sbin/sshd", "-D"]

# 設定啟動時執行的命令
CMD ["code-server", "--auth", "password", "--port", "8080", "--host", "0.0.0.0"]