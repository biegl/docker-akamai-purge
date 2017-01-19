FROM alpine:3.5

MAINTAINER Markus Bürgler <markus.buergler@me.com>

# INSTALL DEPENDENCIES.
RUN apk update && apk add \
    gcc \
    libffi-dev \
    musl-dev \
    openssl-dev \
    python-dev \
    py2-pip && \

    # CLEAR CACHE
    rm /var/cache/apk/* && \

    # INSTALL HTTPIE
    pip install httpie-edgegrid

# COPY THE API CREDENTIALS FILE AS HTTPIE LOOKS
# FOR THE FILE IN ~/.edgerc BY DEFAULT
COPY .edgerc /root/.edgerc