FROM texlive/texlive:latest

# 检查并创建源文件
RUN echo "deb http://mirrors.aliyun.com/debian stable main contrib non-free" > /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/debian stable-updates main contrib non-free" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/debian-security stable-security main contrib non-free" >> /etc/apt/sources.list

# 安装额外需要的包
RUN apt-get update && apt-get install -y \
    pandoc \
    python3 \
    python3-pip \
    zsh \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

# 创建用户
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

# 设置 zsh 为默认 shell
RUN chsh -s /bin/zsh

# 设置工作目录
WORKDIR /workspace

# 设置入口点为 zsh
ENTRYPOINT ["/bin/zsh"] 