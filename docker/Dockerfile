FROM python:3.6-slim


WORKDIR /home

#RUN apk add --update git
RUN apt-get install -y git

COPY docker/requirements.txt . 

RUN pip3 install --no-cache -r requirements.txt

COPY . .
