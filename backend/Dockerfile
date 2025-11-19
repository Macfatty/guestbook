# Build stage - använder Red Hat UBI med Go toolset
FROM registry.access.redhat.com/ubi10/go-toolset:1.22 AS builder

WORKDIR /workspace

# Kopiera Go module-filer
COPY go.mod go.sum ./

# Ladda ner dependencies
RUN go mod download

# Kopiera all källkod
COPY . .

# Bygg applikationen
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o guestbook-api .

# Runtime stage - minimal UBI image
FROM registry.access.redhat.com/ubi10-minimal:10.0

WORKDIR /app

# Kopiera den kompilerade binären från builder
COPY --from=builder /workspace/guestbook-api /app/guestbook-api

# Exponera port (informativt)
EXPOSE 8080

# Kör applikationen
CMD ["/app/guestbook-api"]
