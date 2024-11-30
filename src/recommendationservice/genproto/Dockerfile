# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

FROM python:3.12-slim-bookworm

WORKDIR /build

COPY ./src/recommendationservice/genproto/requirements.txt .

RUN python -m pip install -r requirements.txt
