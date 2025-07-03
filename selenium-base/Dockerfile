FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base

# Обновления и системные зависимости (для запуска Chrome в headless-режиме)
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    unzip \
    gnupg \
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
    && rm -rf /var/lib/apt/lists/*

# Установка Google Chrome
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/google.gpg \
 && echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list \
 && apt-get update && apt-get install -y google-chrome-stable \
 && rm -rf /var/lib/apt/lists/*

# Установка ChromeDriver, соответствующего версии установленного Chrome
RUN CHROME_VERSION=$(google-chrome --version | grep -oP '\d+\.\d+\.\d+') && \
    CHROMEDRIVER_URL=$(curl -sS https://googlechromelabs.github.io/chrome-for-testing/last-known-good-versions-with-downloads.json \
        | grep -A 20 "\"$CHROME_VERSION\"" \
        | grep "linux64.zip" \
        | grep -o 'https://[^"]*') && \
    wget -O /tmp/chromedriver.zip "$CHROMEDRIVER_URL" && \
    unzip /tmp/chromedriver.zip -d /usr/local/bin && \
    rm /tmp/chromedriver.zip && \
    chmod +x /usr/local/bin/chromedriver

# Установка переменных окружения
ENV CHROME_BIN=/usr/bin/google-chrome \
    CHROMEDRIVER=/usr/local/bin/chromedriver
