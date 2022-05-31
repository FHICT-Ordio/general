# Products and learning outcomes

In this document I will showcase the different products I have made during this project, and explain what parts of my deliver cover what outcomes.

<br>

## Table of Content
- [Ordio API application](#ordio-api-application)
- [GitHub](#github)
- [Docker](#docker)
- [API Documentation](#api-documentation)
- [Ordio admin webtool](#ordio-admin-webtool)
- [Auth0](#auth0)
- [Public hosting](#public-hosting)
- [Research](#research)

[Learning Outcomes](#learning-outcomes)

<br>
---
<br>

## Ordio API microservice
The first major product I have made for this project is the main API backend application that feeds the entire Ordio platform with all its data. This API containst two kinds of endpoints meant for two different purposes: Secured and Public access points. 

### Secure access points
The first kind of access point are secure access points. These are any access points that require authorization through Auth0 (which will be touched upon later in the [Auth0](#auth0) chapter). These access points are most of the CRUA access points for the data and are mainly meant to only be accessed by authorized services. These access points provide authorized developers the possibility to manipulate the stored data through API calls.

I say CRUA instead of CRUD because of the nature of the service im providing: Since my platform is a portal for external developers to build applications upon, data is very valuable. Hence I made the decision to not Delete data as CRUD suggest but instead Archive any data. This means no data will be lost which might be important for unforseeable cases in the future.

<br>

### Public access points
The second kind of access point are Public access points. These access points are mainly access points used for accessing data without being able to edit the data the server has stored. These access points are the public access points developers can use to build their applications upon.

<br>

### Accessing the API
To make it easy to access the API endpoints and make future expansion through more microservices possible, all current and future API endpoints are not directly publicly accessible, but instead run through an API gateway. More about this is explain in the [API Gateway](#api-gateway) chapter.

<br>

### Demonstration
Here follows a quick video demonstration of the API. Postman is used to make the API calls for demonstration purposes

<br>

### Outcomes
This product touches learning outcome 1 and 2

<br><br>

## API Gateway
As touched upon earlier, all the Ordio endpoints are not publically accessible. Instead, all request run through one public API gateway. This provides a few key atvantages:
- All API access points are accessible through the same hostname;
- Adding new microservices is easy as new routes can easily be defined in the gateway;
- The gateway allows for call-throttling, DDOS protection and other integrated security protections;

<br>

### Overview
The workings of the gateway can be summerised in this diagram:
![API Gateway Diagram](./Media/API%20Gateway%20Diagram.png)

What you can see in this diagram is the simulated path two requests from clients would take. In my implementation only the Menu service currently exists. However, because of the usage of an API gateway upscaling is very easy: To add more microservices, just define how to reach it in the API gateway and youre ready to go! All microservices are independent of one another and can be easily implemented without any hastle.

<br>

### Outcomes
This product touches learning outcome 1, 2 and 4

<br><br>

## GitHub
After setting up the first version of the API microservice it was time to start working on a way to handle the project files: GitHub. I started by making a [GitHub company](https://github.com/FHICT-Ordio) to bundle all my required repositories together. My goal was to put every microservice, frontend application, the API gateway and every other independent service on its own repository, making they much more sustainable in the long run while still keeping everything in one place.

<br>

### CI/CD
Next up was CI/CD implementation. CI/CD stands for continuous integration and continuous delivery, meaning automation of integration and delivery/deployment of the many different aspects of the project. Every repository of my project runs roughly the same CI/CD process. This process contains a few different key steps:

- #### Build- and unit-testing
The first step of the CI/CD process is quite straight forword: Test if the application can be build successfully in the first place. If so, run all the applications unit tests. Do these also pass? If so, this step has succeeded.

- #### Docker build-testing and deployment
The next step in the CI/CD process is docker testing and possibly deployment. All microservices and standalone applications of the Ordio project run in dockerized containers. More about this will be discusses later in the [Docker](#docker) chapter. Docker requires docker images to build its containers. These images are supplied trough Dockerhub.

Whenever a push or merge to a main branch occurs, the image on Dockerhub needs to be updated with the new most recent code. The CI/CD is responsible for this! First of all, GitHub will test if the dockerimage can be build from the new files in the main branch, using each branches custom made Dockerfiles. If the build succeeds, next the CI/CD process will log into Dockerhub with the credentials provided through GitHub secrets, and push the build docker image to their corresponding Dockerhub registries. If both these steps succeed the new image has successfully been deployed to Dockerhub and is ready for server-sided deployment

- #### Swagger-collection
The next step in the CI/CD process is one only executed for API microservice branches. These branches contain yaml files with definitions for Swagger (Which will be talked about more in the [API Documentation](#swagger) chapter) to use for the generation of API documentation. However, in the case of multiple API microservices, having these Swagger files spread over possibly countless seperate repositories can be very frustrating and makes this documentation near to useless.

What if we could collect all these Swagger files and put them on one general branch then? We can! This step in the CI/CD process is responsible for exactly this: Whenever changes to an API microservice get pushed or merged to one of the repositories main branches, take said yaml file and push it to one main repository, where the independent files get combined into one Swagger interface: the Ordio Swagger portal. This page is hosted on GitHub where independent developers can use it to implement the API. The best part: Whenever any of the code of an API microservice changes, this Swagger portal also automatically updates the API definitions according to the current production code!


<details>
    <summary>GitHub CI/CD Flow File (yaml)</summary>

``` yaml
name: Main

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Setup .NET
      uses: actions/setup-dotnet@v2
      with:
        dotnet-version: 6.0.x
    - name: Restore dependencies
      run: dotnet restore "./menu-service/" 
    - name: Build
      run: dotnet build --no-restore "./menu-service/"
    - name: Test
      run: dotnet test --no-build --verbosity normal "./menu-service/"
    - name: Upload documentation file
      uses: actions/upload-artifact@v3
      with:
        name: docs
        path: ./menu-service/menu-service/api-menu.yaml
        retention-days: 1

  deployment:
    name: deployment
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
      - name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_TOKEN }}
      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          context: ./menu-service/
          file: ./menu-service/menu-service/Dockerfile
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ secrets.DOCKER_HUB_USERNAME }}/menu-service:latest
  
  push-docs:
    runs-on: ubuntu-latest
    needs: build
    if: github.event_name != 'pull_request'
    steps:
      - uses: actions/checkout@v3
        with:
          repository: FHICT-Ordio/general
          token: ${{ secrets.PAT_TOKEN }}
      
      - name: Download docs artifact
        uses: actions/download-artifact@v3
        with:
          name: docs
      
      - name: Check Git status
        id: check
        run: echo "::set-output name=status::$(git status -s)"
      
      - name: Commit files
        id: commit
        if: steps.check.outputs.status != ''
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add .
          git commit -m "Add changes"
          git push

```

</details>

<br>

### Security and code quality
GitHub also provides very useful tools to analyse your code and make sure no vulnerabilities slip through the cracks. My project uses two major technologies that check and notify about exactly this. These are also part of the CI/CD process. These technologies are the following:

- #### CodeQL
CodeQL is a static code analysis tool integrated in GitHub. For my project I set up CodeQL so that whenever a merge request onto any of the main branches is made, GitHub will first check the code quality and vulnerabilities before allowing said code to be pushed at all. Any vulnerabilities are noted and reported to the branches Security panel. If any of the vulnerabilities found are of High risk factor, the merge will be blocked until the vulnerabilities are fixed or the merge is forced through.

- #### Dependabot
The second technology I use to ensure safe code for production environments is Dependabot. Dependabot is another integrated GitHub tool that automatically scans used dependencies of applications and reports if it finds any possible vulnerabilities with these depenencies. All issues found are noted and reported to the brances Security panel, and if any of the vulnerabilities found are of High risk factor, the merge will be blocked until fixed or the merge is forced through. Furthermore, Dependabot will also automatically try to fix any vulnerabilities found by either automatically updating packages for you, or finding different similar packages to use instead.

| CodeQL | Dependabot |
| --- | ---- |
| ![CodeQL](./Media/CodeQL.png) | ![Dependabot](./Media/Dependabot.png) |

<br>

### Outcomes
This product touches learning outcome 2, 3, 4

<br><br>

## Docker
All the applications that are part of the Ordio platform run in a live server environment. This is not in independent single applications, but the entire platform runs in one Docker unit, split in multiple containers: one for each microservice and application.

<br>

### Containers
In total, the platform runs using five unique containers. These containers are the following:

- #### Menu service container
This container hosts the Menu microservice, the API application described in [this](#ordio-api-microservice) chapter;

- #### Admin client container
This container hosts the Admin client application, the frontend application described in [this](#ordio-admin-webtool) chapter;

- #### Gateway container
This container hosts the API gateway application described in [this](#api-gateway) chapter;

- #### Database container
This container hosts the Ordio database. This is an MSSQL database which is responsible for storing all data the Ordio platform needs. The content of this database dynamically updates using Migrations whenever changes to the code structure are made;

- #### Watchtower container
To make sure all the containers stay up do date with the latest code on the main GitHub branches, I use an extension called Watchtower. Watchtower is a dockerised application running next to the other containers which on regular intervals checks if new images of the other appications have been pushed to DockerHub. If so, Watchtower wil automatically restart the container and pull the latest code to make sure the production environment always stays up to date;

<br>

### Compose
All these different containers are started and managed using Docker compose. Docker compose offers great customization that allows for startup sequences to be predefined in contrary to the commands that usually have to individually be entered to start individual containers with vanila docker. In this compose file a few key components are specified:
- Docker image to use for the container
- Volumes for containers for permanent data storage (without, data gets reset on container restart)
- Environmental variables for the container
- Portbinding for container

<details>
    <summary>Docker Compose File (yaml)</summary>

``` yaml
version: "3.8"
services:
  # Support Services
  watchtower:
    container_name: watchtower
    image: containrrr/watchtower
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /etc/timezone:/etc/timezone:ro
    command: --interval 300
  
  gateway:
    container_name: gateway
    image: robinvhoof/api-gateway:latest
    environment:
      # Host Bindings
      Kestrel__Endpoints__Http__Url: "http://+:2000"
      Kestrel__Endpoints__Https__Url: "https://+:1000"
      # SSL Bindings
      ASPNETCORE_Kestrel__Certificates__Default__Password: "letmein"
      ASPNETCORE_Kestrel__Certificates__Default__Path: "/https/aspnetapp.pfx"
    volumes:
      - ~/.aspnet/https:/https:ro
    ports:
      - 8080:1000
    networks:
      - service-network
  
  # Data Services
  db:
    container_name: db
    image: "mcr.microsoft.com/mssql/server:2019-latest"
    environment:
        SA_PASSWORD: "YjxFVK#C8kJZPk4l"
        ACCEPT_EULA: "Y"
    ports:
      - 1433:1433
    volumes:
      - ./mssql:/var/opt/mssql/data
    networks:
      - service-network

  # API Services
  menu-service:
    container_name: menu-service
    image:  robinvhoof/menu-service:latest
    environment:
      # Connection String Injections
      ConnectionStrings__MenuContext: "Server=db;Database=menu;User=sa;Password=YjxFVK#C8kJZPk4l;"
      # Host Bindings
      Kestrel__Endpoints__Http__Url: "http://+:2001"
      Kestrel__Endpoints__Https__Url: "https://+:1001"
      # SSL Bindings
      ASPNETCORE_Kestrel__Certificates__Default__Password: "letmein"
      ASPNETCORE_Kestrel__Certificates__Default__Path: "/https/aspnetapp.pfx"
    volumes:
      - ~/.aspnet/https:/https:ro
    ports:
      - 1001:1001
    depends_on:
      - db
      - gateway
    networks:
      - service-network

  # Frontend Containers
  admin-client:
    container_name: admin-client
    image: robinvhoof/admin-client:latest
    environment:
      # API Host Definitions
      REACT_APP_API_URL: "https://robinvanhoof.tech:1000"
      HTTPS: true
    ports:
      - 3000:3000

# Networking
networks:
  service-network:
    driver: bridge
```

</details>

| Overview | Docker view |
| --- | --- |
| ![Container overview](./Media/Docker%20overview.png) | ![Docker view](./Media/Docker.png) |

<br>

### Outcomes
This product touches learning outcome 1, 3, 4

<br><br>

## API Documentation
Because of the nature of the Ordio platform being a portal for fellow developers to build upon using the APIs, the API documentation is a very big vital part of making sure the project stays alive: Without clear documentation on how to use it, there's no point in keeping the service running if no one knows what to do with it. This is why I put a lot of time into geting the API documentation to a high level which will also be touched upon in my research later in the [Research](#research) chapter.

<br>

### Swagger
The first major component of the API documentation is the Swagger Portal. This Swagger Portal is one place where all the current and future API endpoint are and will be documenten. The endpoint definitions here are set up to be automatically generated from the live code and set to update whenever changes to this live code happend.

Furthermore, the Swagger Portal also offers the tools to directly make calls to either the live production servers or a local test server, which can be used to play around with and test the endpoints. 

The portal itself runs on a GitHub pag and doesnt depend on the APIs or other parts of the platform. This means that even if the APIs were to temporarily go down, developers could still use the information from the Portal to write their code. Take a look at the portal [here](https://fhict-ordio.github.io/general/).

![Swagger](./Media/Swagger.png)

<br>

### Developer Notes
Beside the documentation the Swagger Portal provides more documentation about the API for developers can be found on the Admin Webtool which will be talked about later in the [Ordio Admin Webtool](#ordio-admin-webtool) chapter. The Admin Webtool site has a developer section providing developers with in detail epxplainations on what endpoints to use for what purposes, how to use them, and in addition also giving you JavaScript and C# past-and-go code snippets for general API calls, and endpoint-specific API calls. This developer section can be found [here](https://robinvanhoof.tech/development).

![Developer page](./Media/Developer%20page.png)

<br>

### Outcomes
This product touches learning outcome 1, 4

<br><br>

## Ordio Admin Webtool


<br><br>

## Auth0


<br><br>

## Public Hosting


<br><br>

## Research




<br><br><br>

# Learning Outcomes
The products above all cover parts of the learning outcomes. Below a table summerizing all outcomes and products that show these outcomes:

| Outcome | Products |
| --- | --- |
|  1. Web application: You design and build user-friendly, full-stack web applications. | [Ordio API Microservice](#ordio-api-microservice), [API Gatewayway](#api-gateway), [Docker](#docker), [API Documentation](#api-documentation) |
|  2. Software quality: You use software tooling and methodology that continuously monitors and improve the software quality during software development.  | [Ordio API Microservice](#ordio-api-microservice), [API Gateway](#api-gateway), [GitHub](#github) |
| 3. CI/CD: You implement a (semi)automated software release process that matches the needs of the project context. | [GitHub](#github), [Docker](#docker) |
| 4. Professional: You act in a professional manner during software development and learning. | [API Gateway](#api-gateway), [GitHub](#github), [Docker](#docker), [API Documentation](#api-documentation) |