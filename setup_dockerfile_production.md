# Setup your Dockerfile-production
Add a new file called 'Dockerfile-production', [like this](Dockerfile-production). We name it differently, so that it doesn't conflict with the Dockerfile for Development, that you use on your machine most of the time.
<br>

You could also put all of your Docker files for production into a folder, to be able to call them 'Dockerfile' and 'docker-compose.yml' instead of adding the -production. Then you would need to change the name to access the file in the Github Action.
<br>
<br>

We start from a base image that has ruby 3.1.2 on it with alpine linux for extra speed. In production we also specify the platform of the server, which is amd64.
```
FROM --platform=amd64 ruby:3.1.2-alpine3.16
```
<br>

This makes a folder on the top Level, '/' and calls that Folder 'goclimb'. Later in the file we can reference this folder with a '.' You can call this whatever you want, for example 'app'
```
WORKDIR /goclimb
```
<br>

This installs all necessary dependencies for your Rails Production Environment. It uses 'apk add', the default package manager for Alpine Linux.
```
RUN apk add --update \
            git=2.36.2-r0 \
            libpq-dev=14.5-r0 \
            --no-cache build-base=0.5-r3 \
            --no-cache gcompat=1.0.0-r4 \
            tzdata=2022c-r0 \
            nodejs=16.16.0-r0 \
            yarn=1.22.19-r0
```
<br>

Here we copy the files for yarn and bundler into to working directory(./), aka '/goclimb', and execute yarn install and bundle install
```
# Application dependencies
COPY package.json yarn.lock ./
RUN yarn install
COPY Gemfile Gemfile.lock ./
RUN bundle
```
<br>

Here we're precompiling our assets, to speed up our server.
```
# precompiling assets to make the rails server 🔥fast
COPY . .
RUN rails assets:precompile
```


# This container exposes port 3000 on localhost
EXPOSE 3000
