FROM texlive/texlive:latest

# 设置代理
ENV HTTP_PROXY=http://host.docker.internal:7890
ENV HTTPS_PROXY=http://host.docker.internal:7890

# 创建 apt 目录结构和 sources.list 文件
RUN mkdir -p /etc/apt && \
    echo "deb http://mirrors.ustc.edu.cn/debian bullseye main contrib non-free" > /etc/apt/sources.list && \
    echo "deb http://mirrors.ustc.edu.cn/debian-security bullseye-security main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.ustc.edu.cn/debian bullseye-updates main contrib non-free" >> /etc/apt/sources.list

# 确保不使用其他源
RUN rm -f /etc/apt/sources.list.d/*

# 安装额外需要的包，但不包括 Python 相关包
RUN apt-get clean && apt-get update && apt-get install -y --fix-missing \
    pandoc \
    zsh git curl sudo && rm -rf /var/lib/apt/lists/*

# 创建用户，并设置默认 shell 为 zsh
RUN useradd -m -s /bin/zsh stu
# 设置密码
RUN echo "stu:000" | chpasswd
# 加入 sudo 组
RUN usermod -aG sudo stu

# 切换到 stu 用户
USER stu

# 安装 oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# 安装插件
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting \
    && git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# 配置 zsh
RUN echo 'export ZSH=$HOME/.oh-my-zsh' > ~/.zshrc \
    && echo 'ZSH_THEME="robbyrussell"' >> ~/.zshrc \
    && echo 'plugins=(git zsh-syntax-highlighting zsh-autosuggestions history)' >> ~/.zshrc \
    && echo 'source $ZSH/oh-my-zsh.sh' >> ~/.zshrc \
    && echo 'HIST_STAMPS="yyyy-mm-dd"' >> ~/.zshrc \
    && echo 'HISTSIZE=10000' >> ~/.zshrc \
    && echo 'SAVEHIST=10000' >> ~/.zshrc \
    && echo 'HISTFILE=~/.zsh_history' >> ~/.zshrc

# 设置工作目录
WORKDIR /workspace

# 设置入口点为 zsh
ENTRYPOINT ["/bin/zsh"] 