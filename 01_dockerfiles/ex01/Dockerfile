FROM debian

MAINTAINER ynaamane <ynaamane@student.42.fr>

ENV TSV=3.9.1

RUN apt-get update -y && \
	apt-get upgrade -y && \
	apt-get install -y wget bzip2 && \

ADD https://files.teamspeak-services.com/releases/server/3.9.1/teamspeak3-server_linux_amd64-3.9.1.tar.bz2 ./

RUN tar jxf teamspeak3-server_linux_amd64-$TSV.tar.bz2 && \
	mv teamspeak3-server_linux_amd64 /opt/teamspeak && \
	rm teamspeak3-server_linux_amd64-$TSV.tar.bz2

ADD entrypoint.sh /entrypoint.sh

# 9987 default voice
# 10011 server query
# 30033 file transport
EXPOSE 9987/udp 10011 30033

ENTRYPOINT [ "/entrypoint.sh" ]
