FROM alpine
MAINTAINER ynaamane <ynaamane@student.42.fr>

RUN apk update && \
	apk upgrade -y && \
	apk add vim

ENTRYPOINT vim

# How to build it?
# docker build -t ex00 .

# How to run it?
# docker run --rm -ti ex00
