FROM python:3.12.7-slim

COPY --from=docker.io/library/eclipse-temurin:8-jre /opt/java/openjdk /usr/local/openjdk-8

ENV JAVA_HOME=/usr/local/openjdk-8
ENV PATH="${JAVA_HOME}/bin:${PATH}"

RUN apt-get update \
    && apt-get install -y --no-install-recommends procps git \
    && update-alternatives --install /usr/bin/java java "${JAVA_HOME}/bin/java" 1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY ./services/streaming_processing /app

RUN --mount=type=cache,target=/root/.cache/pip \
    pip install -r streaming_processing_requirements.txt

LABEL Name="streaming_processing"

CMD ["python", "streaming_processing.py"]
