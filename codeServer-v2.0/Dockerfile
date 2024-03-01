# 第一阶段：构建依赖
FROM pytorch/pytorch:2.1.0-cuda11.8-cudnn8-runtime AS builder

RUN python -m pip install --upgrade pip \
    && pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple \
    && pip install jupyterlab \
    && apt-get update \
    && apt-get install -y \
        expect \
        jq \
        curl \
        dumb-init \
    && curl -fOL https://github.com/coder/code-server/releases/download/v4.19.0/code-server_4.19.0_amd64.deb \
    && dpkg -i code-server_4.19.0_amd64.deb

# 第二阶段：创建最终镜像
FROM pytorch/pytorch:2.1.0-cuda11.8-cudnn8-runtime

# 设置环境变量
ENV PATH=/usr/local/bin/:$PATH \
    SHELL=/bin/bash

# 复制构建阶段的依赖到最终镜像
COPY --from=builder /usr/local/lib/python3.8/dist-packages/ /usr/local/lib/python3.8/dist-packages/
COPY --from=builder /usr/local/bin/jupyter-lab /usr/local/bin/jupyter-lab
COPY --from=builder /usr/bin/code-server /usr/bin/code-server

# 复制其他文件
COPY welcome.md edit_config.py /root/
COPY start.sh requirements.txt ./

# 安装Python依赖
RUN pip install --no-cache-dir -r requirements.txt 

# 清理APT缓存
RUN apt-get clean

# CMD
CMD ["./start.sh"]
