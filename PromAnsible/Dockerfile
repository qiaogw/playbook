FROM phusion/baseimage

ENV DEBIAN_FRONTEND noninteractive

RUN    useradd  luhya 

RUN echo "luhya ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers 
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list


RUN apt-get -y  update && \
    apt-get -y upgrade  && \
    apt-get install -y python-pip git cdbs quilt libzmq3-dev ansible sshfs sshpass python-yaml 

RUN echo "[global]" >> ~/.pip/pip.conf 
RUN echo "index-url = http://mirrors.aliyun.com/pypi/simple/" >> ~/.pip/pip.conf 
RUN echo "[install]" >> ~/.pip/pip.conf 
RUN echo "trusted-host = mirrors.aliyun.com" >> ~/.pip/pip.conf 



RUN pip install --upgrade pip  && \
    pip install   pyinstaller pexpect pyzmq 

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