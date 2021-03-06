## Basic Example
Basic Compose configuration for dockerizing a simple Ruby on Rails/SQLite app.
I assume using a debian-based image in the Dockerfile commands.
* [Dockerfile](/basic/Dockerfile)
* [docker-compose.yml](/basic/docker-compose.yml)

### Dockerfile
Choose ruby version from the _[docker images](https://hub.docker.com/_/ruby/)_
```Dockerfile
FROM ruby:2.5
```
Install Node.js
```Dockerfile
RUN apt-get update -qq && \
    apt-get install -y build-essential nodejs
```
Create the app working directory
```Dockerfile
WORKDIR /app
```
Add rails user and give it ownership of the app directory\
The user has a home directory, the default 1000 UID and belongs to a group with the same name
```Dockerfile
RUN useradd -u 1000 -Um rails && \
    chown -R rails:rails /app
```
Set rails as the default user
```Dockerfile
USER rails
```

### docker-compose.yml
Choose compose version
```YAML
version: '3'
```
Set a volume for the gem bundle
```YAML
volumes:
  app_bundle:
```

Configure the web service
```YAML
services:
  web:
    # build the app image from the Dockerfile in the working dir
    build: .
    # start the server binding to all interfaces
    command: rails s -b 0.0.0.0 -p 3000
    # send the right signal to the rails server when stopping the container
    stop_signal: SIGINT
    volumes:
      # bind the working dir on the host machine to the one in the container
      - .:/app
      # use the volume for storing the bundle
      - app_bundle:/usr/local/bundle
    # expose rails server to the host machine
    ports:
      - 3000:3000
    # try restarting the service on failure
    restart: on-failure
```
