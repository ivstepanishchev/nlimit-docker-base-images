FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base

# Установка системных зависимостей
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    unzip \
    gnupg \
    jq \
    libglib2.0-0 \
    libnss3 \
    libgconf-2-4 \
    libfontconfig1 \
    libx11-xcb1 \
    libxcb1 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxi6 \
    libxtst6 \
    libxrandr2 \
    libasound2 \
    libatk-bridge2.0-0 \
    libgtk-3-0 \
    libdrm2 \
    --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Ставим Chrome через .deb архив нужной версии
ENV CHROME_VERSION=114.0.5735.90

RUN wget -O /tmp/chrome.deb https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/$CHROME_VERSION/linux64/chrome-linux64.zip && \
    mkdir -p /opt/chrome && \
    apt-get update && apt-get install -y unzip libxss1 libappindicator3-1 libatk-bridge2.0-0 libgbm1 libu2f-udev && \
    unzip /tmp/chrome.deb -d /opt/chrome && \
    ln -s /opt/chrome/chrome-linux64/chrome /usr/bin/google-chrome && \
    rm /tmp/chrome.deb

# Скачиваем соответствующий ChromeDriver
RUN CHROMEDRIVER_URL=$(curl -sS https://googlechromelabs.github.io/chrome-for-testing/last-known-good-versions-with-downloads.json \
    | jq -r --arg ver "$CHROME_VERSION" '.channels | to_entries[] | select(.value.version == $ver) | .value.downloads.chromedriver[] | select(.platform == "linux64") | .url') && \
    wget -O /tmp/chromedriver.zip "$CHROMEDRIVER_URL" && \
    unzip /tmp/chromedriver.zip -d /usr/local/bin && \
    rm /tmp/chromedriver.zip && \
    chmod +x /usr/local/bin/chromedriver

# Переменные окружения
ENV CHROME_BIN=/usr/bin/google-chrome \
    CHROMEDRIVER=/usr/local/bin/chromedriver
