FROM ubuntu:wily

RUN apt-get -qq update && \
    apt-get -qq install --no-install-recommends \
	build-essential \
	curl \
        ca-certificates \
	git \
	lsof \
	nodejs \
	python \
	vim && \
    apt-get -y clean all && \
    rm -rf /var/lib/apt/lists/* /tmp/*

RUN git clone --depth 1 --single-branch https://github.com/c9/core.git /cloud9
WORKDIR /cloud9
RUN scripts/install-sdk.sh

RUN C9_DIR=$HOME/.c9 && PATH="$C9_DIR/node/bin/:$C9_DIR/node_modules/.bin:$PATH" && cd $C9_DIR && npm install pty.js
RUN sed -i -e 's_127.0.0.1_0.0.0.0_g' /cloud9/configs/standalone.js 

RUN mkdir /workspace
VOLUME /workspace

EXPOSE 80

CMD ["nodejs", "/cloud9/server.js", "-p", "80", "-w", "/workspace"]
