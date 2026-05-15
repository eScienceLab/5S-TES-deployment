FROM golang:1.26-alpine AS builder
ARG FUNNEL_VERSION

RUN apk add --no-cache git make bash
WORKDIR /build
RUN git clone https://github.com/calypr/funnel.git .
RUN git checkout tags/${FUNNEL_VERSION}
RUN go build -o bin/funnel .

FROM docker:dind-rootless

USER root
COPY --from=builder /build/bin/funnel /usr/local/bin/funnel
RUN chown rootless:rootless /usr/local/bin/funnel && chmod +x /usr/local/bin/funnel
USER rootless

# Set data dir
WORKDIR /opt/funnel/data

# Set entrypoint
WORKDIR /opt/funnel
COPY --chmod=+x entrypoint.sh ./entrypoint.sh 
ENTRYPOINT ["./entrypoint.sh"]
