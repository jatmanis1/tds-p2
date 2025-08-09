# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Set the working directory in the container
WORKDIR /app

# Install system dependencies (including Tesseract OCR)
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        tesseract-ocr \
        tesseract-ocr-eng \
        build-essential \
        libglib2.0-0 \
        libsm6 \
        libxext6 \
        libxrender-dev \
        gcc \
    && rm -rf /var/lib/apt/lists/*
# ^ Including build-essential and common libs helps for Pillow, numpy, matplotlib, etc.

# Copy only requirements first (for Docker cache)
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Now copy all the rest of the code (after deps)
COPY . .

# Expose the FastAPI port
EXPOSE 80

# Set environment variable (optional)
ENV NAME World

# ENTRYPOINT for production
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
