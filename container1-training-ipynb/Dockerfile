# Use official Python 3.11 base image
FROM python:3.11.9-slim

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Set working directory
WORKDIR /app

# Install system dependencies (for packages like opencv, pdf2image, pillow etc.)
RUN apt-get update && apt-get install -y \
    gcc \
    git \
    libgl1 \
    poppler-utils \
    ffmpeg \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

# Copy your project files
COPY . .

# Run your main app (adjust this as needed)
CMD ["python", "app.py"]
