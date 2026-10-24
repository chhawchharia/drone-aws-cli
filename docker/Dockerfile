FROM harness0.harness.io/oci/docker_artifacts/hhi/harnessos-alpine:1@sha256:290a37c85b4284d2c11fa0778e5da79643d912a1f5a57d6f701b77644503b7e7 AS hhi-ca-certificates
RUN apk add --no-cache 'ca-certificates>=20261020-r0'

FROM debian:buster-slim@sha256:bb3dc79fddbca7e8903248ab916bb775c96ec61014b3d02b4f06043b604726dc
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
