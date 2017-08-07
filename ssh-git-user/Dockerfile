FROM fedora:latest

RUN dnf -y install openssh-server git
RUN dnf clean all

# setup openssh
RUN sed -i "s/#PasswordAuthentication yes/PasswordAuthentication no/" /etc/ssh/sshd_config
RUN sed -i 's|#PidFile.*$|PidFile /home/git/sshd.pid|' /etc/ssh/sshd_config
RUN sed -i 's/#Port.*$/Port 2022/' /etc/ssh/sshd_config

# SSHd 7.4+ (maybe earlier) this is not needed, see
#  https://lists.mindrot.org/pipermail/openssh-unix-dev/2017-August/036168.html
# RUN sed -i 's/#UsePrivilegeSeparation.*$/UsePrivilegeSeparation no/' /etc/ssh/sshd_config

RUN mkdir /var/run/sshd
RUN rm -f /var/run/nologin

# setup git user
RUN adduser --system -s /bin/bash git
RUN mkdir -p /home/git/.ssh
RUN touch /home/git/.ssh/authorized_keys
RUN chmod 700 /home/git/.ssh
RUN chmod 600 /home/git/.ssh/authorized_keys
RUN ln -s /home/git /repos

# setup sample git repo
RUN mkdir /home/git/sample.git && git -C /home/git/sample.git init --bare

# set some private keys if you wish
#RUN echo 'static key' > /home/git/.ssh/authorized_keys

# make stuff git owned
RUN chown git -R /home/git /etc/ssh

EXPOSE 2022
LABEL Description="sample git server; you need to add your ssh keys after startup; on restart you lose repos by default" Vendor="Red Hat" Version="1.0"

USER git
# CMD ["/usr/sbin/sshd", "-D"]
CMD ssh-keygen -A && exec /usr/sbin/sshd -D
