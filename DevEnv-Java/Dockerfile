FROM devenv:base

USER root

RUN DEBIAN_FRONTEND=noninteractive  && \
	apt-get update && apt-get install -y zip unzip

USER devuser

RUN curl -fsSL https://get.sdkman.io | bash -

WORKDIR /home/devuser/development

# RUN source .skdman/bin/sdkman-init.sh
# RUN sdk install java 17.0.7-tem

