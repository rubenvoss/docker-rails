# Setup you Dockerfile
Add a new file called 'docker-compose.yml', [like this](docker-compose.yml). This file runs your docker container, your PosotgreSQL Database and any other container you want to add.
<br>

Version of docker compose you're working with.
```
version: '3.8'
```
All of the different Docker containers that you app needs are indented once after services:
```
services:
```
This is your PostgreSQL DB container
```
  db_development:
    container_name: db_development
    image: postgres:14.5-alpine3.16
    restart: always
    environment:
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=goClimb_development
    ports:
      - 5432:5432
    volumes:
      - postgres_data:/var/lib/postgresql/data
```
This is your container that runs puma, the rails server.
```
  rails_development_server:
    container_name: rails_development_server
    build: .
    ports:
      - 3000:3000
    restart: always
    depends_on:
      - db_development
    # if you change any code in your files, it will be updated in the container
    volumes:
      - ./:/goclimb:rw
    command: ["rails", "server", "-b", "0.0.0.0"]
```
The cool thing about Docker is, that you can add containers easily, such as a management platform for your Database.
```
  adminer:
    # adminer is a user interface on the web, to display contents of db
    # domain - localhost:8080
    # database system - postgresql
    # server - db_development
    # user - postgres
    # password - password
    # database - postgres
    container_name: adminer
    image: adminer
    restart: always
    ports:
      - 8080:8080
```
This is your volume with the PostgreSQL Database data
```
volumes:
  postgres_data:
```
And now you can start your containers with 'docker compose up'.
