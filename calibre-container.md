# Calibre Download in Docker

The Amazon Kindle reader software and hardware do have a good user experience compared to some stock reader apps (..cough..economist ios app..cough..).

The '''Dockerfile''' 
'''
FROM ubuntu:devel
ENV TZ Europe/Stockholm
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get install -y tzdata
RUN dpkg-reconfigure tzdata
RUN apt-get install -y calibre-bin

ENV LANG en_US.utf8
ENV RECIPE "The Economist"

VOLUME /tmp/calibre

CMD ["bash","-c","ebook-convert \"${RECIPE}\".recipe /tmp/calibre/\"${RECIPE}\".mobi --output-profile kindle"]
'''

Building the image with:
'''docker build -t calibre-container .'''

Run theimage and download the 
'''docker run --name get-economist -v /home/ds/calibre/output:/tmp/calibre -it --rm  calibre-container'''
