FROM nginx

RUN echo deb http://ftp.us.debian.org/debian jessie main > /etc/apt/sources.list

RUN apt-get update --fix-missing
RUN apt-get install -y vim
ADD lhc.conf /etc/nginx/conf.d/lhc.conf

RUN echo "alias l='ls $LS_OPTIONS -lA'" >> /root/.bashrc && \
    echo "alias lsa='ls -alh'" >> /root/.bashrc && \
    echo "alias ll='ls -alh'" >> /root/.bashrc && \
    echo 'export TERM=xterm' >> ~/.bashrc && \
    echo "export VISIBLE=now" >> /etc/profile
