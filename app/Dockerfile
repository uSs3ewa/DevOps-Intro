# Builder stage
FROM golang:1.24-alpine AS builder

# Install ca-certificates for HTTPS requests (if needed)
RUN apk add --no-cache ca-certificates git

# Set working directory
WORKDIR /build

# Copy go.mod and go.sum first for better layer caching
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy the rest of the source code
COPY . .

# Build static binary with stripped symbols and trimmed paths
RUN CGO_ENABLED=0 GOOS=linux go build \
    -ldflags='-s -w' \
    -trimpath \
    -o quicknotes .

# Runtime stage - distroless static with nonroot user
FROM gcr.io/distroless/static:nonroot

# Copy the binary from builder
COPY --from=builder /build/quicknotes /quicknotes

# Expose port 8080
EXPOSE 8080

# Set entrypoint (exec form)
ENTRYPOINT ["/quicknotes"]
