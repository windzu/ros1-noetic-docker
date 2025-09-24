FROM osrf/ros:noetic-desktop-full

# --- 创建非 root 用户 (关键部分) ---
# 接收来自 devcontainer.json 的构建参数
ARG USER_UID=1000
ARG USER_GID=1000
ARG USERNAME=ros

# 创建组和用户
RUN groupadd --gid $USER_GID $USERNAME
RUN useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME

# 授予 sudo 权限
RUN echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$USERNAME
RUN chmod 0440 /etc/sudoers.d/$USERNAME

# (可选) 授予对 video/audio 设备的访问权限
RUN usermod -aG video $USERNAME
RUN usermod -aG audio $USERNAME

# 在切换到普通用户前创建 /workspace 并授予权限
RUN mkdir -p /workspace && chown $USER_UID:$USER_GID /workspace

#  --- 切换到新用户 ---
USER $USERNAME
ENV HOME=/home/$USERNAME

# 为新用户创建工作空间目录
RUN mkdir -p /workspace/src
WORKDIR /workspace

# 9. 在新用户的 .bashrc 中配置 ROS 环境
RUN echo "source /opt/ros/noetic/setup.bash" >> /home/$USERNAME/.bashrc
RUN echo "if [ -f /workspace/devel/setup.bash ]; then source /workspace/devel/setup.bash; fi" >> /home/$USERNAME/.bashrc