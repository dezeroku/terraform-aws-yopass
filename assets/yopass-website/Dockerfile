# --------------------------------------------------------------------- base ---

FROM node:22 AS base

RUN mkdir -p /opt/app
WORKDIR /opt/app

ARG YOPASS_VERSION=16.0.1

RUN git clone https://github.com/jhaals/yopass.git .
RUN if [ "$YOPASS_VERSION" != "latest" ] ; then git checkout $YOPASS_VERSION ; fi

ARG YOPASS_BACKEND_URL="https://my-domain.com"
RUN cd website && \
    yarn install --frozen-lockfile --network-timeout 600000 && \
    YOPASS_BACKEND_URL="$YOPASS_BACKEND_URL" yarn build

# ------------------------------------------------------------------ package ---

FROM alpine:3.22 AS package

COPY --from=base /opt/app/website/dist/ /opt/app/dist/

RUN apk add zip \
    && cd /opt/app/dist \
    && zip -r /tmp/package.zip .
