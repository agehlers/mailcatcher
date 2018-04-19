# mailcatcher
MailCatcher S2I image

# What does mailcatcher do?
MailCatcher runs a super simple SMTP server which catches any message sent to it to display in a web interface. Run mailcatcher, set your favourite app to deliver to smtp://127.0.0.1:1025 instead of your default SMTP server, then check out http://127.0.0.1:1080 to see the mail.

# References:
* Website:  https://mailcatcher.me
* Docker Image: https://hub.docker.com/

# Dockerfile Example:
```
FROM alpine:latest

ENV VERSION 0.6.5
ENV HTTP_PORT 1080
ENV SMTP_PORT 1025

RUN apk add --update ruby ruby-dev ruby-bigdecimal sqlite sqlite-dev build-base libstdc++ ca-certificates && \
    gem install json --no-ri --no-rdoc && \
    gem install mailcatcher -v $VERSION --no-ri --no-rdoc && \
    apk del --purge ruby-dev build-base && \
    rm -rf /var/cache/apk/*

EXPOSE $SMTP_PORT $HTTP_PORT

CMD mailcatcher -f --ip=0.0.0.0 --smtp-port=$SMTP_PORT --http-port=$HTTP_PORT
```
