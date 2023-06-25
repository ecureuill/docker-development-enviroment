FROM devenv:base

USER root

RUN DEBIAN_FRONTEND=noninteractive  && \
	curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo bash - && \
	apt-get install -y nodejs && \
	npm -v && \
	node -v

USER devuser
WORKDIR /home/devuser/development
