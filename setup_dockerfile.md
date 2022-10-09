# Setup you Dockerfile
Add a new file called 'Dockerfile', [like this](Dockerfile)

This is your base Image. This runs your docker container with ruby 3.1.2 in a Alpine Linux container - for exra speed and slimness.
```
FROM ruby:3.1.2-alpine3.16
```
<br>

This makes a folder on the top Level, '/' and calls that Folder 'goclimb'. Later in the file we can reference this folder with a '.'
```
WORKDIR /goclimb
```
<br>

This installs all necessary dependencies for your Rails Development Environment. It uses 'apk add', the default package manager for Alpine Linux.
```
# Operating system dependencies
RUN apk add --update \
            ruby-dev=3.1.2-r0 \
            git=2.36.2-r0 \
            libpq-dev=14.5-r0 \
            --no-cache build-base=0.5-r3 \
            --no-cache gcompat=1.0.0-r4 \
            tzdata=2022c-r0 \
            nodejs=16.16.0-r0 \
            nano \
            yarn=1.22.19-r0
```
<br>

Here we copy the files for yarn and bundler into to working directory, aka '/goclimb', and execute yarn install and bundle install
```
# Application dependencies
COPY package.json yarn.lock ./
RUN yarn install
COPY Gemfile Gemfile.lock ./
RUN bundle
```
<br>

We use the nano texteditor to edit credentials files in the docker container.
```
# You can use the nano texteditor to edit credentials files in the docker container
ENV EDITOR=nano
```

```
# Copy the source code into the Docker container
COPY . .

# This container exposes port 3000 on localhost
EXPOSE 3000
```
<br>

### You should also set up a docker-compose.yml file, to run your PostgreSQL database. If you just want to run the Dockerfile, uncomment this. This is the command that runs the rails server.
```
# Uncomment this if you want to run the docker container without docker compose ($ docker build -t rails .)
# CMD ["rails", "server", "-b", "0.0.0.0"]
```
