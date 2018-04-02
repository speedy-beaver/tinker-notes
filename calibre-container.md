# Calibre Download in Docker

The Amazon Kindle reader software and hardware do have a good user experience compared to some stock reader apps (..cough..economist ios app..cough..)

So I prefer to downloading suitable content with calibre's prepared recipes into ebooks and consume them with any form of kindle reader.

To further simplify the process I have created a dockerfile that will create an container image which by default downloads "The Economist".

## Create the Image
The ```Dockerfile``` with ```ubuntu:latest```:
```
FROM ubuntu:latest
RUN apt-get update && apt-get install -y calibre-bin

ENV RECIPE "The Economist"

VOLUME /tmp/calibre

CMD ["bash","-c","ebook-convert \"${RECIPE}\".recipe /tmp/calibre/\"${RECIPE}\".mobi --output-profile kindle"]
```

The ```Dockerfile``` with ```ubuntu:devel``` :
```
FROM ubuntu:devel

ENV TZ Europe/Stockholm
ENV LANG en_US.utf8
ENV RECIPE "The Economist"

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get install -y tzdata
RUN dpkg-reconfigure tzdata
RUN apt-get install -y calibre-bin

VOLUME /tmp/calibre

CMD ["bash","-c","ebook-convert \"${RECIPE}\".recipe /tmp/calibre/\"${RECIPE}\".mobi --output-profile kindle"]
```
Build the image with:
```docker build -t calibre-container .```

## Use the Image
Run the image: 
```docker run -v /home/speedy-beaver/calibre/output:/tmp/calibre -it --rm  calibre-container```

The ```-v /home/speedy-beaver/calibre/output:/tmp/calibre``` maps the directory ```/home/speedy-beaver/calibre/output``` into the container, the downloaded ebook will be found in that directory. 

By overwriting the RECIPE environment variable, it is possible to trigger any other of the built-in calibre recipes, e.g. like this: 
```docker run -e RECIPE="The Verge" -v /home/speedy-beaver/calibre/output:/tmp/calibre -it --rm  calibre-container```

