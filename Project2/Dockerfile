# Use official Python slim image
FROM python:3.10-slim

# Set working directory
WORKDIR /app


# Install system dependencies for Chrome, Chromedriver, OCR, and Python libs
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    unzip \
    curl \
    gnupg \
    ca-certificates \
    fonts-liberation \
    libglib2.0-0 \
    libnss3 \
    libx11-xcb1 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxfixes3 \
    libxi6 \
    libxtst6 \
    libxrandr2 \
    libasound2 \
    libatk1.0-0 \
    libcups2 \
    libatk-bridge2.0-0 \
    libpangocairo-1.0-0 \
    libxshmfence1 \
    tesseract-ocr \
    tesseract-ocr-eng \
    build-essential \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Add Google Chrome repo and install stable Chrome
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && apt-get install -y google-chrome-stable && \
    rm -rf /var/lib/apt/lists/*

# Install Chromedriver matching installed Chrome version
RUN CHROME_VERSION_FULL=$(google-chrome --version | grep -oE '[0-9]+\.[0-9]+\.[0-9]+') && \
    echo "Detected Chrome version: $CHROME_VERSION_FULL" && \
    CHROMEDRIVER_VERSION=$(curl -sS "https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${CHROME_VERSION_FULL}") && \
    echo "Using Chromedriver version: $CHROMEDRIVER_VERSION" && \
    wget -O /tmp/chromedriver.zip "https://chromedriver.storage.googleapis.com/${CHROMEDRIVER_VERSION}/chromedriver_linux64.zip" && \
    unzip /tmp/chromedriver.zip -d /usr/local/bin/ && \
    rm /tmp/chromedriver.zip && \
    chmod +x /usr/local/bin/chromedriver

# Copy only requirements first (for caching)
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the app code
COPY . .

# Expose the port your FastAPI app listens on
EXPOSE 80

# Run the FastAPI app with uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
