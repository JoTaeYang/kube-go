# 1단계: 빌드 스테이지
FROM golang:1.22.2-alpine AS builder

# 필요 패키지 설치
RUN apk add --no-cache git

# 작업 디렉토리 설정
WORKDIR /app

# go.mod와 go.sum 복사 및 의존성 설치
COPY go.mod go.sum ./
RUN go mod download

# 애플리케이션 소스 복사 및 빌드
COPY . .
RUN go build -o server

# 2단계: 실행 스테이지
FROM alpine:latest

WORKDIR /app

# 루트 인증서 설치 (HTTPS 통신 필요 시)
RUN apk --no-cache add ca-certificates

# 빌드된 실행파일 복사
COPY --from=builder /app/server .

# 서버 포트 노출 (기본 Gin 포트는 8080)
EXPOSE 8080

# 애플리케이션 실행
ENTRYPOINT ["./server"]
