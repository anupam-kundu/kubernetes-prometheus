FROM golang:1.9.2 as builder
RUN go get github.com/crate/crate_adapter
COPY . /go/src/github.com/crate/crate_adapter/
WORKDIR /go/src/github.com/crate/crate_adapter
RUN CGO_ENABLED=0 GOOS=linux go build

FROM alpine:3.7
EXPOSE 9268
COPY --from=builder /go/src/github.com/crate/crate_adapter/crate_adapter .
COPY applications/crate-adapter/config.yml config.yml

ENTRYPOINT ["/bin/sh" , "-c"]
CMD ["./crate_adapter >> crate_adapter.log 2>&1 < /dev/null & sleep 5 && while sleep 5; do grep -i error crate_adapter.log; if [ $? -ne 1 ]; then echo error; break; fi; echo successful;  done "]
