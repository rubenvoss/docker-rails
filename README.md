# Dockerize your Rails App(s)
## What are the benefits of Dockerizing my Railsapps?
- Bugs will be the same on every machine - no more "it works on my side"
- makes it easy to scale a team of developers
- makes it easy to deploy your app to server(s)
<br>

# Development
Using Docker in Development makes sense if you
    - want to learn docker
    - expand your team easily without an "installation day"
    - have the same bugs on every machine
## [1. Setup your Dockerfile](setup_dockerfile.md)
## [2. Setup your docker-compose.yml file](setup_docker_compose.md)
## [3. Setup a run Script](bash_run_script.md)

# Production
Using docker in production makes sense pretty much always
    - easy scaling
    - contained environment
    - less setup
Depending on how your pipeline is set up, you can have your Dockerfile & docker-compose.yml for production on the server or on your Github repo. Here we build the image with Github Actions, so the Dockerfile-production is in the Github repo and the docker-compose.yml file is located on the server.
## [1. Setup your Dockerfile-production](setup_dockerfile_production.md)
## [2. Setup your docker-compose.yml file](setup_docker_compose_production.md)
