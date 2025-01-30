# Build stage
ARG TARGETARCH

FROM alpine:latest AS builder

WORKDIR /app

# Map Docker architectures to TorrServer's release names
RUN case "${TARGETARCH}" in \
    "amd64") TS_ARCH="amd64" ;; \
    "arm64") TS_ARCH="arm64" ;; \
    "arm") TS_ARCH="arm7" ;; \
    *) echo "Unsupported architecture: ${TARGETARCH}"; exit 1 ;; \
    esac && \
    apk add --no-cache curl jq && \
    LATEST_TAG=$(curl -s https://api.github.com/repos/YouROK/TorrServer/releases/latest | jq -r '.tag_name') && \
    curl -sLO "https://github.com/YouROK/TorrServer/releases/download/${LATEST_TAG}/TorrServer-linux-${TS_ARCH}" && \
    chmod +x TorrServer-linux-${TS_ARCH}

# Runtime stage
FROM alpine:latest

WORKDIR /app
COPY --from=builder /app/TorrServer-linux-* /app/TorrServer

EXPOSE 8090 50002 50003

ENTRYPOINT ["/app/TorrServer"]
