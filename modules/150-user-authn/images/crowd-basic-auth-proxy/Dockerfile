ARG BASE_DISTROLESS
ARG BASE_GOLANG_20_ALPINE
FROM $BASE_GOLANG_20_ALPINE as artifact
ARG GOPROXY
ARG SOURCE_REPO

ENV GOPROXY=${GOPROXY} \
    SOURCE_REPO=${SOURCE_REPO} \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

WORKDIR /src/
COPY /app/ /src/
RUN apk add --no-cache git && \
    go build -ldflags="-s -w" ./cmd/crowd-basic-auth-proxy

RUN chown 64535:64535 crowd-basic-auth-proxy
RUN chmod 0700 crowd-basic-auth-proxy

FROM $BASE_DISTROLESS
COPY --from=artifact /src/crowd-basic-auth-proxy /crowd-basic-auth-proxy
ENTRYPOINT [ "/crowd-basic-auth-proxy" ]
