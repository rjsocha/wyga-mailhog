# syntax=docker/dockerfile:1.6
FROM golang:1.18-alpine as builder
# Install MailHog:
RUN apk --no-cache add git binutils \
  && mkdir -p /root/gocode \
  && export GOPATH=/root/gocode \
  && go install github.com/mailhog/MailHog@latest \
  && strip -x /root/gocode/bin/MailHog
RUN mkdir -p /output/mailhog /output/sack \
  && cp /root/gocode/bin/MailHog /output/mailhog
COPY --chmod=755 mailhog-entrypoint /output/mailhog-entrypoint
RUN adduser -D -u 15015 mailhog
RUN mkdir /output/maildir \
    && chown mailhog:mailhog /output/maildir /output/sack \
    && ln -s /mailhog/MailHog /output/mailhog/mailhog

FROM alpine:3 as mold
COPY --from=builder /output/ /
RUN adduser -D -u 15015 mailhog

FROM scratch
COPY --from=mold / /
VOLUME /maildir
WORKDIR /mailhog
ENV PATH "$PATH:/mailhog"
USER mailhog
# Expose only the UI port (for easier Traefik integration)
EXPOSE 8025
ENTRYPOINT ["/mailhog-entrypoint"]
