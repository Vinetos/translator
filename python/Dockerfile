# This image is smaller than the alpine one
FROM python:3.12-slim-bullseye
# Infos

LABEL org.opencontainers.image.authors="valentin.chassignol@epita.fr"
LABEL description="My web Azure translator app"

# Default properties
EXPOSE 5000
ENV FLASK_APP=app.py

# Uncomment fort debug
# RUN apt-get update && apt-get install -y bash

# Install deps
COPY requirements.txt requirements.txt
RUN pip3 install --upgrade -r requirements.txt

# Copy server
COPY .. .

# Add specific user
RUN useradd flask-user
USER flask-user

# Entrypoint + CMD
ENTRYPOINT [ "flask" ]
CMD [ "run", "--host", "0.0.0.0", "--port", "5000" ]