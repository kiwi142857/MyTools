# LaTeX Docker 环境

这是一个基于 texlive/texlive 的 LaTeX 开发环境，集成了常用的 LaTeX 工具和舒适的命令行体验。

## 特性

- 基于官方 texlive 镜像
- 集成 pandoc 文档转换工具
- 配置了 zsh + oh-my-zsh
- 包含语法高亮和命令历史建议
- 使用阿里云镜像源加速包安装

## 环境要求

- Docker
- Make

## 快速开始

1. 构建镜像：
```bash
make build
```

2. 启动容器：
```bash
make start
```

3. 进入容器：
```bash
make shell
```

## 目录挂载

默认会将当前目录挂载到容器的 `/workspace` 目录下。你可以通过 `SRC_DIR` 变量指定要挂载的目录：

```bash
make start SRC_DIR=/path/to/your/latex/files
```

## Makefile 命令说明

- `make build`: 构建 Docker 镜像
- `make start`: 启动容器（后台运行）
- `make stop`: 停止并删除容器
- `make shell`: 进入容器的交互式终端
- `make restart`: 重启容器
- `make clean`: 清理临时文件（.aux, .log, .out 等）

## 容器环境配置

- 默认 shell: zsh
- oh-my-zsh 主题: robbyrussell
- 已安装插件:
  - git
  - zsh-syntax-highlighting（语法高亮）
  - zsh-autosuggestions（命令建议）
  - history（历史记录）

## 注意事项

1. 首次构建镜像可能需要一些时间，因为需要下载基础镜像和安装额外的包
2. 容器会在后台持续运行，使用完毕后记得使用 `make stop` 停止容器
3. 所有在 `/workspace` 目录下的更改都会同步到宿主机的对应目录

## 常见问题

1. 如果遇到权限问题，请检查挂载目录的权限设置
2. 如果需要安装额外的包，可以在容器内使用 `apt-get install`
3. 历史命令保存在容器的 `~/.zsh_history` 文件中

## 自定义配置

如果需要自定义 zsh 配置，可以修改 Dockerfile 中的 .zshrc 配置部分。如果需要安装额外的工具，可以在 Dockerfile 的 apt-get install 部分添加包名。
