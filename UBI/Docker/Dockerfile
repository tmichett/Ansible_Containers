FROM registry.redhat.io/ubi9:latest

ARG USER=devops

# Add sudo and openssh-server
RUN dnf update -y  && dnf install openssh-server sudo -y && dnf clean all

# Setup running user on the container with sudo rights and
# password-less ssh login
RUN useradd -m ${USER}
RUN echo "${USER} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/sudoers

# As the user setup the ssh identity using the key in the tmp folder
USER "${USER}"
RUN mkdir ~/.ssh
RUN chmod -R 700 ~/.ssh
COPY --chown=${USER} id_rsa.pub /home/${USER}/.ssh/id_rsa.pub
RUN cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
RUN chmod 644 ~/.ssh/id_rsa.pub
RUN chmod 644 ~/.ssh/authorized_keys

# start ssh with port exposed
USER root
#RUN service ssh start
RUN ssh-keygen -A
EXPOSE 22
#ENTRYPOINT ["/lib/systemd/systemd"]

CMD ["/usr/sbin/sshd", "-D"]
