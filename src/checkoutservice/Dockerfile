# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM golang:1.22-alpine AS base-build

WORKDIR /build

RUN apk add --no-cache protobuf-dev

RUN go env -w GOMODCACHE=/root/.cache/go-build
COPY ./src/checkoutservice/go.mod ./src/checkoutservice/go.sum ./
RUN --mount=type=cache,target=/root/.cache/go-build go mod download

COPY ./src/checkoutservice/tools.go ./
RUN --mount=type=cache,target=/root/.cache/go-build \
    go list -e -f '{{range .Imports}}{{.}} {{end}}' tools.go | CGO_ENABLED=0 xargs go install -mod=readonly

COPY ./pb ./pb
RUN --mount=type=cache,target=/root/.cache/go-build \
    protoc -I ./pb ./pb/demo.proto --go_out=./ --go-grpc_out=./

COPY ./src/checkoutservice ./
RUN --mount=type=cache,target=/root/.cache/go-build \
    go build -ldflags "-s -w" -o /go/bin/checkoutservice/ ./

FROM alpine

WORKDIR /usr/src/app/

COPY --from=base-build /go/bin/checkoutservice/ ./

EXPOSE ${CHECKOUT_SERVICE_PORT}
ENTRYPOINT [ "./checkoutservice" ]
