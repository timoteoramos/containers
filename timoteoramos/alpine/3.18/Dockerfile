FROM docker.io/alpine:3.18

WORKDIR /srv

ENTRYPOINT [ "/usr/bin/dumb-init", "--" ]

CMD [ "/bin/ash "]

RUN adduser -D -h /srv srv && \
    chown -R srv:srv /srv && \
    apk add dumb-init icu-data-full tzdata
