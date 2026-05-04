FROM golang:1.26.2-alpine3.23 AS builder

WORKDIR /app

# Copy go.mod and go.sum if they exist
COPY go.mod go.sum* ./
RUN if [ -f go.mod ]; then go mod download; fi

# Copy the rest of the source code
COPY . .

# Build the application
# If main.go is in the root directory, this will compile it
RUN if [ -f main.go ]; then go build -o main . ; else touch main; fi

FROM alpine:3.23
WORKDIR /app
COPY --from=builder /app/main .

EXPOSE 8080
CMD ["./main"]
