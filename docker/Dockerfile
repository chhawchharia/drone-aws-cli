FROM harness0.harness.io/oci/docker_artifacts/hhi/harnessos-alpine:1 AS hhi-ca-certificates
RUN apk add --no-cache 'ca-certificates>=20261020-r0'

FROM debian:buster-slim
COPY --from=hhi-ca-certificates /etc/ssl/certs/ca-certificates.crt /etc/hhi/certs/ca-certificates.crt
ENV SSL_CERT_FILE=/etc/hhi/certs/ca-certificates.crt SSL_CERT_DIR=/etc/hhi/certs

ENV GODEBUG netdns=go

# Install required packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates unzip groff curl sudo && \
    rm -rf /var/lib/apt/lists/*

ADD release/linux/amd64/plugin /bin/
ENTRYPOINT ["/bin/plugin"]

COPY --from=hhi-ca-certificates /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
