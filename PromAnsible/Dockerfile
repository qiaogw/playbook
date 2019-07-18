FROM phusion/baseimage

ENV DEBIAN_FRONTEND noninteractive

RUN    useradd  luhya 

RUN echo "luhya ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers 
RUN  sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list


RUN apt-get -y  update && \
    apt-get install -y python-pip git cdbs quilt libzmq3-dev ansible sshfs sshpass python-yaml && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir ~/.pip && \
    cd ~/.pip/  && \
    echo "[global] \ntrusted-host =  pypi.tuna.tsinghua.edu.cn \nindex-url = https://pypi.tuna.tsinghua.edu.cn/simple " >  pip.conf


RUN   pip install   pyinstaller pexpect pyzmq 

RUN  mkdir /src && \ 
    chown luhya:luhya -R /src 
USER luhya
RUN cd /src && \ 
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