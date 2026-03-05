FROM golang:1.26.0-trixie AS build

RUN apt update && apt install -y git nodejs npm

ARG PROMETHEUS_VERSION
ARG HTTP_PROXY
ARG HTTPS_PROXY

ENV HTTP_PROXY="${HTTP_PROXY}"
ENV HTTPS_PROXY="${HTTPS_PROXY}"

RUN git clone https://github.com/prometheus/prometheus.git \
  && cd prometheus \
  && git checkout "${PROMETHEUS_VERSION}"

WORKDIR /go/prometheus

RUN make build

#FROM alpine:3.23.3
FROM scratch

COPY --from=build /go/prometheus/prometheus /prometheus
COPY --from=build /go/prometheus/promtool /promtool

ENTRYPOINT ["/prometheus"]
CMD ["--help"]