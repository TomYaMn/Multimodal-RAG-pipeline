FROM python:3.11.9-slim

# Install system dependencies including sudo
USER root
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    libgl1 \
    poppler-utils \
    ffmpeg \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender-dev \
    tesseract-ocr \
    sudo \
 && rm -rf /var/lib/apt/lists/*

# Copy & filter requirements
COPY requirements.txt /tmp/requirements.txt
RUN grep -vE "^(pywin32|pywinpty)" /tmp/requirements.txt > /tmp/filtered-reqs.txt \
 && pip install --upgrade pip \
 && pip install jupyterlab notebook scipy numpy pandas \
 && pip install -r /tmp/filtered-reqs.txt \
 && rm /tmp/requirements.txt /tmp/filtered-reqs.txt

# Create jovyan user with bash shell
RUN useradd -m -s /bin/bash jovyan

# Set password for jovyan user
RUN echo "jovyan:password" | chpasswd

# Give jovyan sudo permissions without password prompt
RUN echo "jovyan ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/jovyan

# Switch to jovyan user
USER jovyan

WORKDIR /home/jovyan/work
EXPOSE 8888

# Launch JupyterLab without token (local dev only)
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--NotebookApp.token=''"]
