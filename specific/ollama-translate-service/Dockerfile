# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir Flask requests

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Run translate_service.py when the container launches
ENTRYPOINT ["python", "ollama-translate-service.py"]
