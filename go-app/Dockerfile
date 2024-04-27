FROM golang:1.20-alpine AS builder

# Define current working directory
WORKDIR /projectx_api_server

# Download modules to local cache so we can skip re
# downloading on consecutive docker build commands
COPY go.mod .

# Add sources
COPY . .

# Build binary
RUN GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -ldflags="-w -s" -o bin/api-server-bin .

FROM scratch

# Copy binary from builder
COPY --from=builder /projectx_api_server/bin/api-server-bin /
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

# Expose port 8081
EXPOSE 8081

CMD ["/api-server-bin"]