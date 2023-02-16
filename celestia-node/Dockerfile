FROM golang:1.19-alpine AS development
ARG arch=x86_64
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

ARG APP_GIT_TAG
# ENV CGO_ENABLED=0

RUN set -eux; \
    apk add --no-cache \
    git \
    vim \
    tcpdump \
    openssh \
    bash \
    ca-certificates \
    build-base \
    && mkdir -p /build/ 

ARG NODE_GIT_TAG

ENV GOPROXY=https://goproxy.cn,direct

# RUN git clone https://github.com/celestiaorg/celestia-node.git \
#     && cd celestia-node \
#     && git checkout tags/${NODE_GIT_TAG} \
#     && make build \
#     && make cel-key \
#     && cp build/celestia /build/ \
#     && cp cel-key /build/

RUN git clone https://github.com/celestiaorg/celestia-node.git \
    && cd celestia-node \
    && git checkout tags/${NODE_GIT_TAG} \
    && make build \
    && make cel-key \
    && cp build/celestia /build/ \
    && cp cel-key /build/

#----------------------------#

FROM alpine:latest AS production

RUN apk add curl bash jq

WORKDIR /app/
COPY --from=development /build .
COPY ./start.sh ./

ENV BIN_PATH="/app/celestia"
ENV BIN_CELKEY_PATH="/app/cel-key"

ENTRYPOINT ["/app/start.sh"]