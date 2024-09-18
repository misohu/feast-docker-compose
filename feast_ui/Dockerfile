# Use an official Python image as the base
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Install necessary Python packages
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

# Expose port 8888 for Feast UI
EXPOSE 8888

# Command to start the Feast UI
CMD ["feast", "ui"]
