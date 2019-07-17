FROM phusion/baseimage



RUN    useradd  luhya 

RUN echo "luhya ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers 
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN  apt-get clean

ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get -y update && \
    apt-get install -y tzdata  && \
    dpkg-reconfigure --frontend noninteractive tzdata


RUN apt-get install -y python-pip cdbs quilt libzmq3-dev ansible sshfs sshpass python-yaml && \
    pip install pyinstaller pexpect pyzmq && \
    apt-get clean 

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