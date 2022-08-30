# Build Stage
FROM golang:alpine AS build-stage

LABEL app="build-GoAppTestTemplate1"
LABEL REPO="https://github.com/BalaNarvar/GoAppTestTemplate1"

ENV PROJPATH=/go/src/github.com/BalaNarvar/GoAppTestTemplate1

# Because of https://github.com/docker/docker/issues/14914
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin

ADD . /go/src/github.com/BalaNarvar/GoAppTestTemplate1
WORKDIR /go/src/github.com/BalaNarvar/GoAppTestTemplate1

RUN make build-alpine

# Final Stage
FROM alpine-base-image:latest

ARG GIT_COMMIT
ARG VERSION
LABEL REPO="https://github.com/BalaNarvar/GoAppTestTemplate1"
LABEL GIT_COMMIT=$GIT_COMMIT
LABEL VERSION=$VERSION

# Because of https://github.com/docker/docker/issues/14914
ENV PATH=$PATH:/opt/GoAppTestTemplate1/bin

WORKDIR /opt/GoAppTestTemplate1/bin

COPY --from=build-stage /go/src/github.com/BalaNarvar/GoAppTestTemplate1/bin/GoAppTestTemplate1 /opt/GoAppTestTemplate1/bin/
RUN chmod +x /opt/GoAppTestTemplate1/bin/GoAppTestTemplate1

# Create appuser
RUN adduser -D -g '' GoAppTestTemplate1
USER GoAppTestTemplate1

ENTRYPOINT ["/usr/bin/dumb-init", "--"]

CMD ["/opt/GoAppTestTemplate1/bin/GoAppTestTemplate1"]
