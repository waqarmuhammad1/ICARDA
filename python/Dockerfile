FROM python:3

WORKDIR /var/www/html

RUN wget https://packages.couchbase.com/releases/couchbase-release/couchbase-release-1.0-6-amd64.deb
RUN apt-get update && apt-get upgrade -y && \
 apt-get install -y lsb-release \
 apache2 apache2-doc \
 apache2-utils \
 software-properties-common && \
 add-apt-repository -yr ppa:libreoffice/ppa && \
 apt-get update && \
 apt-get install -y libreoffice \
 poppler-utils

RUN apt-cache policy lsb-release
RUN dpkg -i couchbase-release-1.0-6-amd64.deb
RUN apt-get update && apt-get install -y libcouchbase-dev build-essential python-dev python-pip

RUN pip3 install couchbase \
 flask \
 -U flask-cors \
 flask-restful \
 xlrd \
 sklearn \
 scipy \
 pandas \
 numpy \
 mysql-connector-python \
 docx2txt
RUN python3 -m pip install opencv-python opencv-contrib-python
RUN rm -rf /var/lib/apt/lists/*
RUN wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
RUN rm couchbase-release-1.0-6-amd64.deb
RUN bash Anaconda3-2019.03-Linux-x86_64.sh -b -p /root/anaconda3
RUN rm Anaconda3-2019.03-Linux-x86_64.sh

RUN eval "$(/root/anaconda3/bin/conda shell.bash hook)"
RUN . /root/.bashrc
RUN /root/anaconda3/bin/conda init
RUN /root/anaconda3/bin/conda create -n imageai -c conda-forge python=3.6
RUN /root/anaconda3/bin/conda install -c conda-forge tensorflow keras mkl-service opencv numpy scipy pillow matplotlib h5py
RUN a2enmod rewrite && echo "ServerName localhost" >> /etc/apache2/apache2.conf && a2enmod proxy_http && apachectl -k graceful

RUN pip3 install https://github.com/OlafenwaMoses/ImageAI/releases/download/2.0.3/imageai-2.0.3-py3-none-any.whl
RUN pip3 install --upgrade tensorflow keras

ENTRYPOINT /bin/bash -c "exec python3 predator/HSAPI.py & exec python3 predator/RunAPI.py & service apache2 stop && /usr/sbin/apache2ctl -D FOREGROUND"
