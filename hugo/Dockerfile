FROM busybox AS build-env
ENV HUGO_VERSION=0.54.0
RUN wget "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz"
RUN tar zxf "hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz"

FROM gcr.io/distroless/cc
ENTRYPOINT ["/hugo"]
COPY --from=build-env /hugo /