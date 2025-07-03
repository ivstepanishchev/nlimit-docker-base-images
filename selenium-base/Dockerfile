FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base

# Установка системных зависимостей (для headless Chrome)
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

# Установка Google Chrome (stable)
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor > /etc/apt/trusted.gpg.d/google.gpg && \
    echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && apt-get install -y google-chrome-stable --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# Получение и установка соответствующего ChromeDriver
RUN CHROME_VERSION=$(google-chrome --version | grep -oP '\d+\.\d+\.\d+') && \
    CHROMEDRIVER_URL=$(curl -s https://raw.githubusercontent.com/GoogleChromeLabs/chrome-for-testing/main/known-good-versions-with-downloads.json | \
      jq -r --arg ver "$CHROME_VERSION" '
        .versions[] 
        | select(.version == $ver) 
        | .downloads.chromedriver[] 
        | select(.platform == "linux64") 
        | .url') && \
    wget -O /tmp/chromedriver.zip "$CHROMEDRIVER_URL" && \
    unzip /tmp/chromedriver.zip -d /usr/local/bin && \
    rm /tmp/chromedriver.zip && \
    chmod +x /usr/local/bin/chromedriver

# Переменные окружения
ENV CHROME_BIN=/usr/bin/google-chrome \
    CHROMEDRIVER=/usr/local/bin/chromedriver
