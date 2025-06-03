FROM ubuntu:25.10

ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        libapache2-mod-wsgi-py3 \
        python3 \
        python3-pip \
        python3-dev \
        python3-venv \
        build-essential \
        libpq-dev \
        curl && \
    apt-get clean

# Create and activate virtual environment
RUN python3 -m pip install --upgrade pip --break-system-packages

ENV PATH="/opt/venv/bin:$PATH"

# Copy Django project files
COPY app/ /app/

# Set working directory
WORKDIR /app

RUN python3 -m venv .venv \
    && . .venv/bin/activate && \
    pip install -r requirements.txt --break-system-packages

# Set working directory
WORKDIR /app

# Apache config
COPY wsgi.conf /etc/apache2/sites-available/000-default.conf

# Enable Apache mods
RUN a2enmod wsgi

# Expose port 80
EXPOSE 80

# Start Apache in foreground
CMD ["apachectl", "-D", "FOREGROUND"]