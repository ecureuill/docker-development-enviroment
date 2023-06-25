FROM debian:bookworm-slim

RUN DEBIAN_FRONTEND=noninteractive \
	apt-get update \
	&& apt-get install -y sudo curl wget nano locales git-core git gnupg zsh fonts-powerline \
	&& adduser --quiet --disabled-password --home /home/devuser --shell /bin/zsh --gecos "User" devuser  \
	&& echo devuser:pass | chpasswd  \
	&& usermod -aG sudo devuser \
	&& rm -rf /var/lib/apt/lists/* \
	&& localedef -i pt_BR -c -f UTF-8 -A /usr/share/locale/locale.alias pt_BR.UTF-8

USER devuser

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" 
	# && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  \
	# && git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions  \
	# && git clone https://github.com/z-shell/F-Sy-H.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/F-Sy-H  \
	# && git clone --depth 1 -- https://github.com/marlonrichert/zsh-autocomplete.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autocomplete


WORKDIR /home/devuser/development
ENV LANG=pt_BR.utf8
ENV ZSH_THEME=agnoster

ADD .zshrc /home/devuser/
ADD *.gitignore /home/devuser/development
ADD *.gitconfig /home/devuser

USER root

RUN chown devuser:devuser /home/devuser/.zshrc

USER devuser

CMD ["zsh"]
