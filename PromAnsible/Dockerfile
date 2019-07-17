FROM alpine


RUN    addgroup -S luhya && \
    adduser -S -D -g "" -G luhya     luhya 

RUN echo "luhya ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers 
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

ENV TZ=Asia/Shanghai

RUN  apk update && \
    apk upgrade && \
    apk  --no-cache add tzdata zeromq  python python-dev python-pip git cdbs quilt libzmq3-dev ansible sshfs sshpass python-yaml   pip install pyinstaller pexpect pyzmq && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo '$TZ' > /etc/timezone && \
    rm -rf /var/cache/apk/* 

RUN mkdir /src && cd /src && \ 
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
    chown luhya:luhya -R /src && \ 
    ansible-playbook playbook/monitor.yml -i hosts -u luhya -kK --extra-vars "myhost=monitor" && \
    ansible-playbook playbook/monitor.yml -i hosts -u luhya -kK --extra-vars "myhost=monitor"


EXPOSE 80 