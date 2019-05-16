FROM liuweipeng/deepo:latest
MAINTAINER liuweipeng

ENV DEBIAN_FRONTEND noninteractive
ENV USER ubuntu
ENV HOME /home/$USER

# Create new user for vnc login.
RUN adduser $USER --disabled-password

# Install Ubuntu Unity.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ubuntu-desktop \
        unity-lens-applications \
        gnome-panel \
        metacity \
        nautilus \
        gedit \
        xterm \
        sudo

# Install dependency components.
RUN apt-get install -y \
        supervisor \
        net-tools \
        curl \
#        git \
        pwgen \
        libtasn1-3-bin \
        libglu1-mesa \
    && apt-get autoclean \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/*

# 安装语言包
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list \
    && apt-get clean && apt-get update
RUN apt-get install -y language-pack-zh-hans-base \
    && locale-gen zh_CN.UTF-8 \
    && export LC_ALL='zh_CN.UTF-8' \
    && echo export LC_ALL='zh_CN.UTF-8'>>/etc/bash.bashrc
ENV LANG zh_CN.UTF-8

# 安装中文字体
COPY MicrosoftYaHei.ttf /usr/share/fonts/windows/
RUN chmod 777 /usr/share/fonts/windows/MicrosoftYaHei.ttf \
    && mkfontscale && mkfontdir && fc-cache -fv

# Copy tigerVNC binaries
ADD tigervnc-1.8.0.x86_64 /

# Clone noVNC.
RUN git clone https://github.com/novnc/noVNC.git $HOME/noVNC

# Clone websockify for noVNC
Run git clone https://github.com/kanaka/websockify $HOME/noVNC/utils/websockify


# Copy supervisor config
COPY supervisor.conf /etc/supervisor/conf.d/

# Set xsession of Unity
COPY xsession $HOME/.xsession

# Copy startup script
COPY startup.sh $HOME

EXPOSE 6080 5901 4040
CMD ["/bin/bash", "/home/ubuntu/startup.sh"]