# Use official Python slim image
FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Install dependencies for Chrome, Chromedriver, Tesseract OCR, and other libs
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget unzip gnupg2 curl ca-certificates fonts-liberation libglib2.0-0 libnss3 libx11-xcb1 libxcomposite1 libxcursor1 libxdamage1 libxi6 libxtst6 libxrandr2 libasound2 libatk1.0-0 libcups2 libpangocairo-1.0-0 libxss1 xdg-utils libgbm1 libpango-1.0-0 libpangocairo-1.0-0 tesseract-ocr tesseract-ocr-eng build-essential gcc libsm6 libxext6 libxrender-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Google Chrome (pinned version)
ENV CHROME_VERSION=114.0.5735.90

RUN wget -q -O /tmp/google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb \
    && dpkg -i /tmp/google-chrome-stable_current_amd64.deb || apt-get -fy install \
    && rm /tmp/google-chrome-stable_current_amd64.deb

# Install matching Chromedriver
ENV CHROMEDRIVER_VERSION=114.0.5735.90

RUN wget -O /tmp/chromedriver.zip https://chromedriver.storage.googleapis.com/${CHROMEDRIVER_VERSION}/chromedriver_linux64.zip \
    && unzip /tmp/chromedriver.zip -d /usr/local/bin/ \
    && rm /tmp/chromedriver.zip \
    && chmod +x /usr/local/bin/chromedriver

# Copy requirements first to leverage Docker cache
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy app code
COPY . .

# Expose port (for FastAPI)
EXPOSE 80

# Command to run your app
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
