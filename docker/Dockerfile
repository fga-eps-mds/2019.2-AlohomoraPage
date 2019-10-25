FROM python:3.6-alpine


WORKDIR /home

RUN apk add --update git

COPY docker/requirements.txt . 

RUN pip3 install --no-cache -r requirements.txt

COPY . .
