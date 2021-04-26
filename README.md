# Project 4 Part 1
CEG 3120 
Due 4/9/2021 at 11:59

Nawras Mustafa
## Project Overview
This project had us dockerize and html file as a first step and then had configure AWS CLI, Create ECR repo and use github actions.

# Part 1

## Installing docker + dependencies:
to install docker I downloaded docker for windows from the offecial site then I ran the following commands in my WSL2 console:

 `sudo apt-get install apt-transport-https ca-certificates curl software-properties-common`

For downloading dependencies

## Building and runing a Container:
I created a Dockerfile in my repo and added these lines to the Dockerfile 

`FROM httpd:2.4
COPY /html/ /usr/local/apache2/htdocs/`

to build a container I used the command in powershell in the repo location:

`docker build -t image:1 .`

and to run it I used the command:

`docker run -d -p 80:80 image:1 `

to view my website I used firefox and typed `localhost:80` in the link field to view the page.

# Part 2  AWS CLI And DockerHub

## Install AWS CLI
I used the 64bit installer form [here](https://aws.amazon.com/cli/)

## Configure AWS CLI
To configure I used `aws configure` in powershell and it prompt me to enter the aws information provided in pilot.

## Install DockerHub
on DockerHub website I made an account and signed in after that I can create a new repo by following this [link](https://hub.docker.com/repositories/create) I made sure to give it a name and make it public.

## Github Secrets
after creating a repo on Dockerhub I went to the project repo and added 2 secrets 
```
DOCKER_USERNAME = { Your Username on DockerHub }
DOCKER_PASSWORD = { Your Password on DockerHub }
```

## Workflows
this is used to automate the push and build process.
we need to first create a workflow at the location `.github/workflows/docker.yml`

after creating the file add the following lines: 
```
name: Publish Docker image
on:
  release:
    types: [published]
jobs:
  push_to_registry:
    name: Push Docker image to Docker Hub
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repo
        uses: actions/checkout@v2
      - name: Push to Docker Hub
        uses: docker/build-push-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
          repository: cryptonic9/dockerp4
          tag_with_ref: true
```
this is a template that I only edited the `repository:` field to match my docker repository.






