FROM python:2.7-slim
RUN apt-get update -qqy && \
	apt-get -qqy install wget g++ git && \
	rm -rf /var/lib/apt/lists/*
# show python logs as they occur
ENV PYTHONUNBUFFERED=0

# download the grpc health probe
RUN GRPC_HEALTH_PROBE_VERSION=v0.2.0 && \
    wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 && \
    chmod +x /bin/grpc_health_probe

# get packages
WORKDIR /recommendationservice
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
# check out the tip of ben-hackery, but bust the Docker cache.
RUN pip install git+https://github.com/benpbrown/ls-trace-py.git@6710dcd546ed8ce002d6022df4f76a08aca85746
RUN pip install opentracing

# add files into working directory
COPY . .

# set listen port
ENV PORT "8080"
EXPOSE 8080

ENTRYPOINT ["ls-trace-run", "python", "/recommendationservice/recommendation_server.py"]
