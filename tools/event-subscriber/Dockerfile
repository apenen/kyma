FROM golang:1.13 as builder

ENV GO111MODULE=on

RUN apt-get install -y ca-certificates

ARG DOCK_PKG_DIR=/event-subscriber

WORKDIR $DOCK_PKG_DIR
COPY . $DOCK_PKG_DIR
COPY ./licenses/ /app/licenses

RUN go mod download

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o event-subscriber ./cmd

FROM scratch
LABEL source=git@github.com:kyma-project/kyma.git

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

COPY --from=builder $DOCK_PKG_DIR/event-subscriber .
COPY --from=builder /app/licenses /app/licenses

CMD ["/event-subscriber"]
