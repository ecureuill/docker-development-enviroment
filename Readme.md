My collection of dockerfiles to dockerize my development enviroment.

1. `devenv:base`  bookworm-slm minimal installation, includes:
    1. oh-my-zsh with agnoster theme
    2. devuser user
    3. my gitconfig
    4. a few gitignore files 
1. `devenv:nodejs` installed nodejs on top of `devenv:base` 
1. `devenv:ruby` installed rbenv on top of `devenv:base`

# How to create images

## Create `devenv:base` image

```bash
cd DevEnvBase
docker build -t devenv:base .
```

## Create `devenv:nodejs` image

```bash
cd DevEnv-Nodejs
docker build -t devenv:nodejs .
```

## Create `devenv:ruby` image

```bash
cd DevEnv-Ruby
docker build -t devenv:ruby .

docker run -it devenv:ruby --name ruby

## while on container
rbenv install 3.2.2
rbenv global 3.2.2
gem install rails
```

on another terminal, update devenv:ruby running `docker commit`
```bash
docker commit ruby devenv:ruby
```

After the commit, devenv:ruby image includes ruby 3.2.2 and rails. 

# How I run my containers

1. create a docker network
```bash
sudo docker network create myappnet
```
2. Run some database container within network

```bash
sudo docker run --rm --name pgsqldev -v ${pwd}/data:/var/lib/postgresql/data --env-file .env -dp 5433:5433 --net myappnet postgres:14.2-alpine
```

3. Run my project container within network

```bash
sudo docker run --name some-app-project \
--rm -it --mount type=bind,source="$(pwd)"/app,target=/home/devuser/development \
--net myappnet --publish 3000:3000 \
devenv:nodejs"
```


And to avoid write down those commands every time, I created a few Terminal profiles

![image](https://github.com/ecureuill/docker-development-enviroment/assets/993369/4464683d-f457-499b-8411-1b2c825c75bd)
