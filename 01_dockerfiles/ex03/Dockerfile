FROM debian:stable
MAINTAINER ynaamane <ynaamane@student.42.fr>

ENV DEBIAN_FRONTEND=noninteractive
ENV TERM=xterm
ARG GITLAB_IP=localhost

RUN apt-get update -y && \
	apt-get upgrade -y && \
	apt-get install -y curl openssh-server ca-certificates git tzdata

# Run GitLab script deployment
RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash

# Install GitLab package
RUN apt-get update -y && apt-get install -y --no-install-recommends gitlab-ce

# Create self-signed certificate
RUN mkdir -p /etc/gitlab/ssl && \
	chmod 700 /etc/gitlab/ssl && \
	openssl req -new -x509 -nodes -newkey rsa:2048 -keyout /etc/gitlab/ssl/gitlab.key -out /etc/gitlab/ssl/gitlab.crt -days 365 -subj /CN=${GITLAB_IP} && \
	sed -i -e "s|^\(external_url\) .*$|\1 'https://${GITLAB_IP}'|" \
	-e "s|^# \(nginx\['ssl_certificate'\] = \"/etc/gitlab/ssl\)/.*$|\1/gitlab.crt\"|" \
	-e "s|^# \(nginx\['ssl_certificate_key'\] = \"/etc/gitlab/ssl\)/.*$|\1/gitlab.key\"|" \
	-e "s|^# \(gitlab_rails\['gitlab_shell_ssh_port'\] = 22\)$|\1|" \
	/etc/gitlab/gitlab.rb

EXPOSE 443
EXPOSE 22

ENTRYPOINT  service ssh start && \
				(/opt/gitlab/embedded/bin/runsvdir-start &) && \
				gitlab-ctl reconfigure && \
				gitlab-ctl tail

# Build
# docker build --build-arg GITLAB_IP=(docker-machine ip Char) -t gitlab .

# Run
# docker run --rm -it -p 8000:443 -p 8022:22 --privileged gitlab

# Test
# open https://(docker-machine ip Char):8000

# Clone HTTPS
# env GIT_SSL_NO_VERIFY=true git clone https://(docker-machine ip Char):8000/root/<repo>.git

# Push HTTPS
# env GIT_SSL_NO_VERIFY=true git push

# Clone SSH
# ssh-keygen -t ed25519 -C 'root@gitlab.com' -f id_ed25519
# pbcopy < id_ed25519.pub
# ssh -T -i id_ed25519 -p 8022 git@(docker-machine ip Char)
# env GIT_SSH_COMMAND='ssh -i id_ed25519 -p 8022' git clone git@(docker-machine ip Char):root/<repo>.git

# Push SSH
# env GIT_SSH_COMMAND='ssh -i ../id_ed25519 -p 8022' git push
