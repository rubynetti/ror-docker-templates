## Base Example
Basic Compose configuration for dockerizing a basic Ruby on Rails app.
I assume using a debian-based image in the Dockerfile commands.

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
Create app directory
```Dockerfile
RUN mkdir /app
```
Set the app directory as the working directory
```Dockerfile
WORKDIR /app
```
Add rails user and give it ownership of the app directory\
The user has a home directory, the default 1000 uid and belongs to a group with the same name.
```-u 1000``` 
```Dockerfile
RUN useradd -u 1000 -Um rails && \
    chown -R rails:rails /app
```
Set rails as the default user
```Dockerfile
USER rails
```

### docker-compose.yml
