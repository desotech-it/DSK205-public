FROM golang:alpine AS builder
RUN apk update && apk add --no-cache git
COPY src $GOPATH/src/prometheus-app-example
WORKDIR $GOPATH/src/prometheus-app-example
RUN go get -d -v
RUN go build -o /tmp/app-example *.go

FROM alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup && mkdir -p /app 
COPY --from=builder /tmp/app-example /app
RUN chmod a+rx /app/app-example
USER appuser
WORKDIR /app
CMD ["./app-example"]