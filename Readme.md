My collection of dockerfiles to dockerize my development enviroment.

1. `devenv:base`  bookworm-slm minimal installation, includes:
    1. oh-my-zsh with agnoster theme
    2. devuser user
    3. my gitconfig
    4. a few gitignore files 
1. `devenv:nodejs` installed nodejs on top of `devenv:base` 
1. `devenv:ruby` installed rbenv on top of `devenv:base`
1. `devenv:sdman`installed sdkman on top of `devenv:base`

# How to create images

1. First create `devenv:base` image

    ```bash
    cd DevEnvBase
    docker build -t devenv:base .
    ```
1. Create others image
    - `devenv:nodejs` image

        ```bash
        cd DevEnv-Nodejs
        docker build -t devenv:nodejs .
        ```
    - `devenv:ruby` image

        ```bash
        cd DevEnv-Ruby
        # create image
        docker build -t devenv:ruby .

        # run container
        docker run -it devenv:ruby --name ruby

        ## (while on container) install 
        rbenv install 3.2.2
        rbenv global 3.2.2
        gem install rails
        ```
        on another terminal, update devenv:ruby running `docker commit`
        ```bash
        docker commit ruby devenv:ruby
        ```
        After the commit, devenv:ruby image includes ruby 3.2.2 and rails. 

    - `devenv:sdkman` image

        ```bash
        cd DevEnv-Java
        docker build -t devenv:base .
        ```



# How I run my containers

1. create a docker network
    ```bash
    docker network create myappnet
    ```

1. create a volume to use with java/sdkman container
    ```bash
    docker volume create java-sdkman-vol
    ```

1. Run some database container within network

    ```bash
    docker run --name pgsqldev -v ${pwd}/data:/var/lib/postgresql/data --env-file .env -dp 5433:5433 --net myappnet --ip 172.18.0.12 postgres:14.2-alpine
    ```

    ```bash
    docker run --name mongodb -v ${pwd}/data:/data/db --env-file .env -dp 27017:27017 --net myappnet --ip 172.18.0.11 mongo:latest
    ```

    ```bash
    docker run --name mysqldev -v ${PWD}/data:/var/lib/mysql --env-file .env -dp 3306:3306 --net myappnet --ip 172.18.0.10 mysql:debian
     ```
1. Run my project container within network
    
    !!!info
        move to the folder thats is going to bind to container before run it (ie: project folder `cd ~/Dev/nodejs.proj/`)  
    
    
    - Java project

        !!!Warning
            I binded a volume to the .sdkman folder to avoid install java, maven, etc every time I run the container 

        ```bash
        # run container
        docker run --name java --rm -it \ 
        --mount type=bind,source="$(pwd)",target=/home/devuser/development \
        --mount source=java-sdkman-vol,target=/home/devuser/.sdkman
        --net myappnet --ip 172.18.0.4 --publish 8080:8080 \
        devenv:sdkman
        ```


    - Node project
        ```bash
        # run container
        docker run --name some-app-project --rm -it \ 
        --mount type=bind,source="$(pwd)",target=/home/devuser/development \
        --net myappnet --ip 172.18.0.1 --publish 3000:3000 \
        devenv:nodejs
        ```
    - React project
        ```bash
        # run container
        docker run --name some-app-project --rm -it \ 
        --mount type=bind,source="$(pwd)",target=/home/devuser/development \
        --net myappnet --ip 172.18.0.2 --publish 3000:3000 \
        devenv:nodejs
        ```
    - Ruby project
        ```bash
        # run container
        docker run --name some-app-project --rm -it \ 
        --mount type=bind,source="$(pwd)",target=/home/devuser/development \
        --net myappnet --ip 172.18.0.3 --publish 3000:3000 \
        devenv:ruby
        ```

1. And to avoid write down those commands every time, I created a few Terminal profiles

    ![image](https://github.com/ecureuill/docker-development-enviroment/assets/993369/4464683d-f457-499b-8411-1b2c825c75bd)
