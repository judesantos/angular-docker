# AngularApp 

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 1.6.7.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `-prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).


# Dockerized Multi-stage deployment

## Setup

### Install Angular CLI:

    $ npm install -g @angular/cli@version

### Create Angular app:

    $ ng new project-name
    $ cd project-name

## Docker

### Add Dockerfile into project root:

    see ./Dockerfile

### Add .dockerignore file with content:

    node_modules
    .git

### Build and Tag Docker image:

    $ docker build -t angular-app

### Run container after the build is done:

    $ docker run -it -v ${PWD}:/usr/src/app -v /usr/src/app/node_modules -p 4200:4200 --rm angular-app

### Checkout http://localhost:4200 in browser. Update html files and see if hot-reload works.
### Kill the server:

    docker container stop container_id

### docker run flags:
    
    -it - interactive with terminal
    -d  - run container in background

### Unit tests. Run Chrome in headless mode using Karma. 
### in karma.config.conf

    browsers: ['ChromeHeadless'],
    customLaunchers: {
        'ChromeHeadless': {
            base: 'Chrome',
            flags: ['--no-sandbox', '--headless', '--disable-gpu', '--remote-debugging-port=9222']
        }
    },

### Run unit test and e2e tests:

    $ docker exec -it angular-app-container ng test --watch=false
    $ ng e2e

### After test. Remote containers:

    $ docker stop angular-app-container
    $ docker rm angular-app-container

### Using docker-compose:
### Add a docker-compose.yaml file on the project root.

    See docker-compose.yaml of this project

### Build image and run container:

    $ docker-compose up -d --build

### Check if app is running in browser. Test hot-reloading.

    $ docker-compose run angular-app ng-test --watch=false
    $ ng e2e

### Stop container before moving to next step:

    $ docker-compose stop

## Production Deployment

### 2 stages: 
### Stage 1 - build and test. Will stop deployment if build or test fails.
### Stage 2 - Copy build files to production.

### Create production docker file Dockerfile-prod:

    See project Dockerfile-prod

### Build and tag production image:

    $ docker build -f Dockerfile-prod -t angular-app-prod .

### Run container:

    $ docker run -it -p 80:80 --rm angular-app-prod

### Using docker-compose:
    
    See docker-compose-prod.yaml

### Run the container:

    $ docker-compose -f docker-compose-prod.yaml up -d --build

