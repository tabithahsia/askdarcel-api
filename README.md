# askdarcel-api [![Build Status](https://travis-ci.org/ShelterTechSF/askdarcel-api.svg?branch=master)](https://travis-ci.org/ShelterTechSF/askdarcel-api)

This project exposes the API endpoints for supporting the askdarcel-web project, which is built using a Ruby on Rails API Server

## Docker-based Development Environment (Recommended)

### Requirements

Docker Community Edition (CE) >= 17.06
Docker Compose >= 1.18

Download and install the version of [Docker for your OS](https://www.docker.com/community-edition#/download).

### Set up the project

This is not a full guide to Docker and Docker Compose, so please consult other
guides to learn more about those tools.

```sh
# Build (or rebuild) Docker images
$ docker-compose build

# Start the database container (in the background with -d)
$ docker-compose up -d db

# Generate random database fixtures
$ docker-compose run --rm api rake db:setup db:populate

# Start the Rails development server in the api container (in the foreground)
$ docker-compose up api

# Stop all containers, including background ones
$ docker-compose stop
```

### Running Postman tests from the command line

```sh
# Reset DB with initial database fixtures
$ docker-compose run --rm api rake db:setup db:populate

# Run Docker container that executes Postman CLI tool named newman
$ docker-compose run --rm postman
```

### Alternative database setup

```sh
# Populate the database with an old dump of the database (circa mid-2017)
$ docker-compose run --rm api rake db:create db:schema:load linksf:import
```

## macOS-based Development Environment Not Using Docker

### Install Dependencies

1. [Install Homebrew](http://brew.sh/).
2. Install [rbenv](https://github.com/rbenv/rbenv) and [ruby-build](https://github.com/rbenv/ruby-build#readme).
  - `brew install rbenv`
    + Follow further setup instructions (including updating your bash
      profile) from the link above.
    + Add the following lines to your `~/.bash_profile`
    ```
    export PATH="$HOME/.rbenv/bin:$PATH"
    eval "$(rbenv init -)"
    ```
  - `brew install ruby-build`
3. Install postgres.
  - `brew install postgresql`
    + Follow further setup instructions displayed after installation.
  - `brew services start postgres`


### Set up the project

After cloning the repository and `cd`ing into the workspace:

1. Install the required ruby version.
  - `rbenv install`
2. Install the `bundle` gem if it isn't yet installed.
      - `which bundle || gem install bundle`
3. Install the required gems.
  - `bundle install`
  If encounter "command not found" error, run
  -`source ~/.bash_profile`

4. Set up the development database and load dummy data.
  - `rake db:create:all`
  - `rake db:migrate`
  - `rake linksf:import`

  Alternatively, you can generate random fixtures:
  - `rake db:setup db:populate`
5. Run the development server.
  - `rails s -b 0.0.0.0`
6. Do NOT do sudo install -rails
