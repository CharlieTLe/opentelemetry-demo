# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

FROM golang:1.23-alpine

WORKDIR /build

RUN apk add --no-cache protobuf-dev

COPY ./src/productcatalogservice ./

RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=cache,target=/root/.cache/go-build \
    go list -e -f '{{range .Imports}}{{.}} {{end}}' tools.go | CGO_ENABLED=0 xargs go install
