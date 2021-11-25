FROM arm64v8/python:3.6-alpine3.13

RUN apk add --no-cache supervisor tcl binutils alpine-sdk postgresql-client postgresql-dev redis curl git bash libffi-dev cmake libssh

WORKDIR /root

# backwards compatibility
# RUN ln -s /usr/local/lib/pyenv/versions/3.6.8/bin/python /usr/local/bin

COPY requirements.txt ./requirements.txt
RUN pip --no-cache-dir install -r requirements.txt --ignore-installed

RUN mkdir -p /etc/artemis/ && \
    mkdir -p /var/log/artemis/

COPY entrypoint Makefile wait-for ./
COPY core ./core

RUN make clean && make -j

ENTRYPOINT ["./entrypoint"]
