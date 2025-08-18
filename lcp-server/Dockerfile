FROM golang:1.23-bookworm AS builder
WORKDIR /src

RUN set -eux; \
    export DEBIAN_FRONTEND=noninteractive; \
    apt-get update && apt-get install -y --no-install-recommends \
      build-essential pkg-config libsqlite3-dev ca-certificates \
      && rm -rf /var/lib/apt/lists/*

COPY go.mod go.sum ./
RUN go mod download
COPY . .

ENV CGO_ENABLED=1 GOOS=linux GOARCH=amd64
RUN go build -trimpath -ldflags="-s -w" -o /out/lcpserver ./cmd/lcpserver

FROM debian:bookworm-slim
RUN set -eux; \
    export DEBIAN_FRONTEND=noninteractive; \
    apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates libsqlite3-0 \
      && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY --from=builder /out/lcpserver /app/lcpserver
EXPOSE 8081
ENTRYPOINT ["/app/lcpserver"]