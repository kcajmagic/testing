FROM docker.io/library/golang:1.14-alpine as builder

MAINTAINER Jack Murdock <jack_murdock@comcast.com>

WORKDIR /go/src/github.com/xmidt-org/testing

ARG VERSION=unknown
ARG GITCOMMIT=unknown
ARG BUILDTIME=unknown


RUN apk add --no-cache --no-progress \
    ca-certificates \
    make \
    git \
    openssh \
    gcc \
    libc-dev \
    upx

COPY . .
RUN make build

FROM scratch

COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /go/src/github.com/xmidt-org/testing/testing.yaml /testing.yaml
COPY --from=builder /go/src/github.com/xmidt-org/testing/testing /testing
COPY --from=builder /go/src/github.com/xmidt-org/testing/deploy/Dockerfile /go/src/github.com/xmidt-org/testing/NOTICE /go/src/github.com/xmidt-org/testing/LICENSE /go/src/github.com/xmidt-org/testing/CHANGELOG.md /

EXPOSE 8080

USER nobody

ENTRYPOINT ["/testing"]
