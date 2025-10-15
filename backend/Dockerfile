FROM python:3.12-slim
WORKDIR /app

# Upgrade pip
RUN pip install --upgrade pip

# Copy requirements and install
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy backend code
COPY /app ./app

# Expose port
EXPOSE 5000

# Correct module path for Uvicorn
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "5000"]

