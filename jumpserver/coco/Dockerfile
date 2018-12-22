FROM python:3.6-alpine3.6
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories && apk update

ARG VERSION=master

WORKDIR /app/coco

RUN set -xe \
 && apk add --no-cache krb5-dev sshpass libffi-dev libressl-dev linux-headers

ADD requirements.txt .

RUN set -xe \
 && apk add --no-cache --virtual .build-deps build-base  \
 && pip install -i https://mirrors.aliyun.com/pypi/simple/ -r requirements.txt \
 && apk del .build-deps

RUN set -xe \
 && apk add curl bash \
 && curl -sSL -o - https://github.com/jumpserver/coco/archive/${VERSION}.tar.gz | tar zxf - \
 && mv coco-${VERSION}/* /app/coco \
 && rm -rf coco-${VERSION}


ADD conf.py .

VOLUME /app/coco/logs
VOLUME /app/coco/keys

EXPOSE 2222

CMD [ "python", "run_server.py" ]
