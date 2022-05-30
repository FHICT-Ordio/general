# Products and learning outcomes

In this document I will showcase the different products I have made during this project, and explain what parts of my deliver cover what outcomes.

<br>

---

## Table of Content
- [Ordio API application](#ordio-api-application)
- [GitHub CI/CD](#github-cicd)
- [Docker](#docker)
- [API Documentation](#api-documentation)
- [Ordio admin webtool](#ordio-admin-webtool)
- [Auth0](#auth0)
- [Public hosting](#public-hosting)
- [Research](#research)
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

<br>

### Security and code quality
GitHub also provides very useful tools to analyse your code and make sure no vulnerabilities slip through the cracks. My project uses two major technologies that check and notify about exactly this. These are also part of the CI/CD process. These technologies are the following:

- #### CodeQL
CodeQL is a static code analysis tool integrated in GitHub. For my project I set up CodeQL so that whenever a merge request onto any of the main branches is made, GitHub will first check the code quality and vulnerabilities before allowing said code to be pushed at all. Any vulnerabilities are noted and reported to the branches Security panel. If any of the vulnerabilities found are of High risk factor, the merge will be blocked until the vulnerabilities are fixed or the merge is forced through.

- #### Dependabot
The second technology I use to ensure safe code for production environments is Dependabot. Dependabot is another integrated GitHub tool that automatically scans used dependencies of applications and reports if it finds any possible vulnerabilities with these depenencies. All issues found are noted and reported to the brances Security panel, and if any of the vulnerabilities found are of High risk factor, the merge will be blocked until fixed or the merge is forced through. Furthermore, Dependabot will also automatically try to fix any vulnerabilities found by either automatically updating packages for you, or finding different similar packages to use instead.

<br>

### Outcomes
This product touches learning outcome 2, 3, 4

<br><br>

## Docker

<br><br>

## API Documentation
### Swagger

<br><br>

## Ordio admin webtool


<br><br>

## Auth0


<br><br>

## Public hosting


<br><br>

## Research