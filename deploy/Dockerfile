FROM golang:1.25-alpine as builder

ENV CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

WORKDIR /dir

COPY go.mod go.sum ./
RUN go mod download

# 这个. 当执行docker build命令时所在的目录是backend/branch-usage
COPY . .

# 下载依赖
RUN go build -ldflags "-s -w" -o test ./cmd/server.go


FROM alpine:latest

WORKDIR /app
COPY --from=builder /dir/test .

EXPOSE 8080

CMD ["./test"]