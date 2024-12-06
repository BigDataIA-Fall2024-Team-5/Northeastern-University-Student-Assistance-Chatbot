# Use the official Airflow image as the base image
FROM apache/airflow:2.10.2

# Switch to root user to install additional packages
USER root

# Set environment variable to avoid interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Update package lists and install required dependencies for Chrome
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    unzip \
    gnupg \
    curl \
    fonts-liberation \
    libasound2 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libatspi2.0-0 \
    libcairo2 \
    libcups2 \
    libdbus-1-3 \
    libdrm2 \
    libgbm1 \
    libglib2.0-0 \
    libgtk-3-0 \
    libnspr4 \
    libnss3 \
    libpango-1.0-0 \
    libx11-6 \
    libxcb1 \
    libxcomposite1 \
    libxdamage1 \
    libxext6 \
    libxfixes3 \
    libxrandr2 \
    xdg-utils \
    libcurl4 \
    libexpat1 \
    libgcc1 \
    libgconf-2-4 \
    libgdk-pixbuf2.0-0 \
    libpangocairo-1.0-0 \
    libstdc++6 \
    libx11-xcb1 \
    libxcursor1 \
    libxi6 \
    libxrender1 \
    libxss1 \
    libxtst6 \
    ca-certificates \
    libappindicator1 \
    lsb-release \
    libvulkan1 \
    && rm -rf /var/lib/apt/lists/*

# Download and install a specific version of Google Chrome
RUN wget https://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_130.0.6723.58-1_amd64.deb \
    && dpkg -i google-chrome-stable_130.0.6723.58-1_amd64.deb || apt-get -f install -y \
    && rm google-chrome-stable_130.0.6723.58-1_amd64.deb

# Download and install ChromeDriver matching the installed Chrome version
RUN wget https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/130.0.6723.58/linux64/chromedriver-linux64.zip \
    && unzip chromedriver-linux64.zip \
    && mv chromedriver-linux64/chromedriver /usr/local/bin/ \
    && chmod +x /usr/local/bin/chromedriver \
    && rm -rf chromedriver-linux64 chromedriver-linux64.zip

# Clean up unnecessary files to reduce image size
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Switch back to airflow user and working directory
USER airflow
WORKDIR /opt/airflow

# Install additional Python dependencies
RUN pip install --no-cache-dir \
    requests==2.32.3 \
    boto3==1.35.68 \
    selenium==4.26.1 \
    beautifulsoup4==4.12.3 \
    pandas==2.2.3 \
    snowflake-connector-python==3.12.3 \
    pinecone-client==5.0.1 \
    langchain-nvidia-ai-endpoints==0.3.5 \
    langchain-pinecone==0.2.0 \
    python-dotenv==1.0.1

# Expose port 8080 (Airflow UI)
EXPOSE 8080
