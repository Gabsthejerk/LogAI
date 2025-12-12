# Use a lightweight Python image
FROM python:3.9-slim

# Install system tools (curl, etc if needed)
RUN apt-get update && apt-get install -y docker.io

LABEL maintainer="LogAI"
LABEL description="Docker and system log analyzer with local LLM integration"

# Set working directory
WORKDIR /app

# Copy requirements and install them
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the actual script
COPY log_analyzer.py .

# Expose API port
EXPOSE 8765

# Run the API server
CMD ["python", "log_analyzer.py"]

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import requests; requests.get('http://localhost:8765/health')"

# Run the application
CMD ["uvicorn", "log_analyzer:app", "--host", "0.0.0.0", "--port", "8765"]