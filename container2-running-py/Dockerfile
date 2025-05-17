# Use official Python image as base
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Copy local files to container
COPY . .

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip and install Python dependencies
RUN pip install --upgrade pip

# Install your required Python packages
RUN pip install \
    streamlit \
    langchain \
    langchain-openai \
    langchain-community \
    langchain-chroma \
    chromadb \
    openai

# Set environment variables (optional, adjust as needed)
ENV STREAMLIT_SERVER_PORT=8501
ENV STREAMLIT_SERVER_ENABLECORS=false
ENV PYTHONUNBUFFERED=1

# Expose Streamlit default port
EXPOSE 8501

# Run Streamlit app
CMD ["streamlit", "run", "app.py"]
