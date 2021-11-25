FROM arm64v8/python:3.6-alpine3.13

RUN apk add --no-cache supervisor tcl binutils alpine-sdk postgresql-client postgresql-dev redis curl git bash libffi-dev cmake libssh

WORKDIR /root
# Install rtlib
RUN git clone -b v0.7.0 --depth 1 https://github.com/rtrlib/rtrlib.git && \
    cd rtrlib && \
    cmake -D CMAKE_BUILD_TYPE=Release . && \
    make -j 4 && make install && \
    ln -s /usr/local/lib64/librtr.so /usr/local/lib/librtr.so && \
    ln -s /usr/local/lib64/librtr.so.0 /usr/local/lib/librtr.so.0 && \
    ln -s /usr/local/lib64/librtr.so.0.7.0 /usr/local/lib/librtr.so.0.7.0 && \
    git clone https://github.com/rtrlib/python-binding.git && \
    cd python-binding && \
    git checkout 2ade90eddd1895515948481136c8197e9cada128 && \
    python setup.py build install

# backwards compatibility
# RUN ln -s /usr/local/lib/pyenv/versions/3.6.8/bin/python /usr/local/bin

COPY requirements.txt ./requirements.txt
RUN pip --no-cache-dir install -r requirements.txt --ignore-installed

RUN mkdir -p /etc/artemis/ && \
    mkdir -p /var/log/artemis/

COPY entrypoint hasura_init.json Makefile wait-for ./
COPY migrate ./migrate
COPY core ./core

RUN make clean && make -j

ENTRYPOINT ["./entrypoint"]
