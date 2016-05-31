FROM fedora:latest

RUN dnf -y install openssh-server git

# setup openssh
RUN sed -i "s/#PasswordAuthentication yes/PasswordAuthentication no/" /etc/ssh/sshd_config
RUN mkdir /var/run/sshd

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
RUN chown git -R /home/git

EXPOSE 22
LABEL Description="sample git server; you need to add your ssh keys after startup; on restart you lose repos by default" Vendor="Red Hat" Version="1.0"
# CMD ["/usr/sbin/sshd", "-D"]
CMD ssh-keygen -A && exec /usr/sbin/sshd -D
