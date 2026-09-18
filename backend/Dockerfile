# syntax=docker/dockerfile:1
FROM python:3.10-slim

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    (apt-get update || apt-get update) && apt-get install -y --no-install-recommends \
    -o Acquire::Retries=5 \
    -o Acquire::http::No-Cache=true \
    -o Acquire::BrokenProxy=true \
    -o Acquire::http::Pipeline-Depth=0 \
    libgl1 \
    libglib2.0-0

COPY requirements.docker.txt .
RUN --mount=type=cache,target=/root/.cache/pip \
    pip install -r requirements.docker.txt

COPY . .

EXPOSE 8020

CMD ["sh", "-c", "python manage.py migrate && python manage.py runserver 0.0.0.0:8020"]
