FROM devenv:base

USER root

COPY get-pip.py .
COPY Anaconda/ /opt/anaconda

RUN apt-get update && \
	DEBIAN_FRONTEND=noninteractive && \
	apt-get upgrade -y && \
	apt-get install -y build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev libbz2-dev && \
	# download python
	curl -fsSL -o python.tar.xz https://www.python.org/ftp/python/3.11.4/Python-3.11.4.tar.xz && \
	# compile python
	tar -xf python.tar.xz && \
	cd Python-3.11.4 && \
    sudo ./configure --enable-optimizations && \
    sudo make install && \
	python3 get-pip.py && \
	pip install flake8 && \
	# 		psycopg2-binary flask-sqlalchemy Flask-Migrate flask-cors python-dotenv 		pytz Flask-bcrypt flask-jwt-extended
	# anaconda microserver
	chown -R devuser:devuser /opt/anaconda/anaconda_server/docker/start && \
	chmod +x /opt/anaconda/anaconda_server/docker/start

USER devuser
WORKDIR /home/devuser/development

CMD ["zsh"]