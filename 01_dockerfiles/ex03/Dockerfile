FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive

ENV TERM xterm

RUN apt-get update && apt-get install -y \
    xterm \
    curl \
    openssh-server \               
    ca-certificates \
    postfix \
    tzdata

RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash

RUN apt-get install gitlab-ce

ARG ip

RUN echo 'external_url "https://'${ip}'"' >> /etc/gitlab/gitlab.rb
RUN echo "letsencrypt['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "prometheus_monitoring['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "grafana['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "unicorn['worker_timeout'] = 300" >> /etc/gitlab/gitlab.rb
RUN echo "gitlab_rails['gitlab_shell_ssh_port'] = 8022" >> /etc/gitlab/gitlab.rb
RUN mkdir -p /opt/gitlab/sv/sshd/supervise \
    && mkfifo /opt/gitlab/sv/sshd/supervise/ok \
    && printf "#!/bin/sh\nexec 2>&1\numask 077\nexec /usr/sbin/sshd -D" > /opt/gitlab/sv/sshd/run \
    && chmod a+x /opt/gitlab/sv/sshd/run \
    && ln -s /opt/gitlab/sv/sshd /opt/gitlab/service \
    && mkdir -p /var/run/sshd
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN echo "Port 8022" >> /etc/ssh/sshd_config
RUN echo "Port 8022" >> /etc/ssh/ssh_config

WORKDIR /etc/gitlab/ssl

RUN openssl req -x509 -sha256 -newkey rsa:2048 -nodes \
    -keyout ${ip}.key -days 365 -out ${ip}.crt -subj '/CN='${ip}

WORKDIR /

EXPOSE 443 8022

CMD (service ssh restart) && (opt/gitlab/embedded/bin/runsvdir-start &) && (sysctl vm.overcommit_memory=1 &) && gitlab-ctl reconfigure && gitlab-ctl tail