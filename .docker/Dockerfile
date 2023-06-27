# Use the Python base image
FROM python:3.11.2-bullseye AS builder

# Define the version of Poetry to install (default is 1.4.2)
ARG POETRY_VERSION=1.4.2

# Install Poetry globally
RUN pip install --upgrade pip && \
    pip install poetry==${POETRY_VERSION}

# Test if Poetry is installed in the expected path
RUN echo "Poetry version:" && poetry --version

# Install basic utils
RUN apt-get update && \
    apt-get install -y tmux vim

# Copy data needed during build
COPY agi_perception /app/agi_perception
COPY README.md pyproject.toml /app/

# Set the working directory for the app
WORKDIR /app

# Regenerate poetry lock, in case toml file changed
RUN poetry lock

# Install dependencies
RUN poetry install --no-interaction --no-ansi --with test

# Clone and install GroundingDINO directly after other dependencies are installed
RUN git clone https://github.com/IDEA-Research/GroundingDINO.git && \
    poetry run pip install ./GroundingDINO

