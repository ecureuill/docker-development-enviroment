FROM devenv:base

USER root

RUN DEBIAN_FRONTEND=noninteractive \
	&& apt update && sudo apt install -y git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev \
	&& apt clean

USER devuser

RUN curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash \
	&& echo 'export PATH="/home/devuser/.rbenv/bin:$PATH"' >> /home/devuser/.zshrc

RUN echo 'eval "$(rbenv init - zsh)"' >> /home/devuser/.zshrc 	

# Build image
# Run container
# Exec
#	rbenv install 3.2.2 \
# 	&& rbenv global 3.2.2 \
# 	&& gem install rails
# Docker commit devenv:ruby