# 默认的源文件目录，可以通过命令行参数覆盖
SRC_DIR ?= $(PWD)

# Docker 镜像名称
IMAGE_NAME = latex-env
CONTAINER_NAME = latex-container

# 构建 Docker 镜像
.PHONY: build
build:
	docker build -t $(IMAGE_NAME) .

# 启动 Docker 容器（后台运行）
.PHONY: start
start:
	docker run -d \
		--name $(CONTAINER_NAME) \
		-v $(SRC_DIR):/workspace \
		$(IMAGE_NAME) -c "tail -f /dev/null"

# 停止并删除容器
.PHONY: stop
stop:
	docker stop $(CONTAINER_NAME)
	docker rm $(CONTAINER_NAME)

# 进入容器
.PHONY: shell
shell:
	docker exec -it $(CONTAINER_NAME) /bin/bash

# 重启容器
.PHONY: restart
restart: stop start

# 清理临时文件
.PHONY: clean
clean:
	docker run --rm \
		-v $(SRC_DIR):/workspace \
		$(IMAGE_NAME) -c "rm -f *.aux *.log *.out" 