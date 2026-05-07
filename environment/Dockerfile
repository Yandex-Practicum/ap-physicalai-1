# Dockerfile — GPU-образ с CUDA, PyTorch, MuJoCo Playground
FROM nvidia/cuda:12.8.0-cudnn-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# ── Core packages ──────────────────────────────────────────────
RUN apt-get update && apt-get install -y \
    # Virtual framebuffer + X11
    xvfb \
    x11vnc \
    x11-xserver-utils \
    # Window manager (lightweight)
    fluxbox \
    # noVNC (browser-based VNC client)
    novnc \
    python3-websockify \
    # Python + libs
    python3 \
    python3-pip \
    # MuJoCo / OpenGL dependencies
    libgl1-mesa-glx \
    libgl1-mesa-dev \
    libglu1-mesa \
    libglu1-mesa-dev \
    libosmesa6-dev \
    libglew-dev \
    libglfw3 \
    libglfw3-dev \
    libegl1-mesa-dev \
    # Utilities
    wget \
    curl \
    net-tools \
    supervisor \
    git \
    && rm -rf /var/lib/apt/lists/*

# ── PyTorch (CUDA 12) ────────────────────────────────────────
RUN pip3 install --no-cache-dir \
    torch --index-url https://download.pytorch.org/whl/cu124

# ── MuJoCo Playground + RSL-RL ───────────────────────────────
RUN pip3 install --no-cache-dir \
    "playground[learning]"

RUN pip3 install --no-cache-dir --upgrade "jax[cuda12]"

# ── Дополнительные зависимости ────────────────────────────────
RUN pip3 install --no-cache-dir \
    mujoco \
    gymnasium[mujoco] \
    numpy \
    imageio[ffmpeg] \
    pytest \
    tensorboard

# ── Environment variables ──────────────────────────────────────
ENV DISPLAY=:99
ENV MUJOCO_GL=glx
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=all

# ── Supervisor config (manages all processes) ──────────────────
COPY environment/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

WORKDIR /workspace

EXPOSE 5900 6080

ENTRYPOINT []
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
