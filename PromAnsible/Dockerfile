FROM frolvlad/alpine-glibc:alpine-3.5


RUN  adduser   luhya   -D   -u 1000 -s /bin/sh
USER luhya
RUN sudo echo "https://mirrors.ustc.edu.cn/alpine/v3.5/main" > /etc/apk/repositories
RUN sudo echo " https://mirrors.ustc.edu.cn/alpine/v3.5/community" >> /etc/apk/repositories 

RUN sudo apk update && \
    apk upgrade && \
    apk add python  python-pip git cdbs quilt libzmq3-dev ansible sshfs sshpass python-yaml  && \
    pip install pyinstaller pexpect pyzmq && \
    rm -rf /var/cache/apk/* 

RUN mkdir ~/src && cd ~/src && \
    git clone https://github.com/cloudfirst/PromAnsible.git && \
    git clone https://github.com/cloudfirst/promansible-Install.git && \
    git clone https://github.com/cloudfirst/sample-ansible-script-for-promansible.git && \
    cd PromAnsible/ && \
    dpkg-buildpackage && \
    make publish && \
    echo " [educloud]" >> /etc/ansible/hosts && \
    echo " monitor ansible_ssh_host=125.75.8.155" >> /etc/ansible/hosts && \
    cd .. && \
    cd promansible-Install && \
    ansible-playbook playbook/monitor.yml -i hosts -u luhya -kK --extra-vars "myhost=monitor" && \
    ansible-playbook playbook/monitor.yml -i hosts -u luhya -kK --extra-vars "myhost=monitor"


EXPOSE 80 