# Используем готовый образ с Rust
FROM mcr.microsoft.com/devcontainers/rust:1-bullseye

# Переключаемся на пользователя root для установки пакетов
USER root

# Устанавливаем Node.js, зависимости для Tauri и VNC-сервер
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    # Зависимости Node.js
    curl \
    # Зависимости Tauri для Debian/Ubuntu
    libwebkit2gtk-4.0-dev \
    build-essential \
    wget \
    file \
    libssl-dev \
    libgtk-3-dev \
    libayatana-appindicator3-dev \
    librsvg2-dev \
    # VNC и оконный менеджер
    supervisor \
    tigervnc-standalone-server \
    novnc \
    fluxbox \
    # Чистка
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Устанавливаем Node.js с помощью nvm (уже есть в базовом образе)
RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && nvm install --lts && nvm use --lts"

# Копируем конфигурацию для VNC сервера
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Возвращаемся к стандартному пользователю
USER vscode

# Устанавливаем Tauri CLI и pnpm
RUN npm install -g pnpm && cargo install tauri-cli

# Устанавливаем пароль для VNC (пароль будет "vscode")
RUN mkdir -p /home/vscode/.vnc && echo "vscode" | vncpasswd -f > /home/vscode/.vnc/passwd && chmod 600 /home/vscode/.vnc/passwd