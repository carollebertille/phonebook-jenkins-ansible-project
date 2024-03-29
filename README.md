# Full integration chain DevSecOps
## Tools 

- Cloud : AWS EC2
- Container Engine : Docker
- Configuration Managment: Ansible
- Scheduling : Jenkins
- Security: Clair, Sonarqube, Gauntlt
- Notification: Slack, Email

## Project 

### The context
                
The objective of this project is to deploy the web application by creating a complete DevSecOps-. For this application to be functional, it is necessary to deploy a MySQL database (backend) and a frontend.
In this project, the problems of businesses, related to storage, to the control of their data and processes were taken into account.

### Infrastructure

#### Description
 
We wanted to reproduce an enterprise-type infrastructure with 4 servers:
- A master type server which will contain the main applications and tools (Jenkins, Ansible, Docker…)
- A build server to build our artifact, do unit tests and security scan of images
- A staging / pre-production server in order to carry out tests of our artifacts in same conditions of production,
- A production server in order to deploy our web application which can be consumed.

#### Infrastructure Diagram



### Choice and description of tools

+ DevOps:
  + Infrastructure deployed on the AWS cloud provider thanks to CloudFormation in order to favor IaC.
  + Using Docker to containerize the database, web application as well as the frontend in two different containers to favor agility.
  + Use of Ansible to configure our infrastructure and provisione it.
  + Implementation of a Gitflow to respect good practices. Creation of two branches:
    + The “main” branch which will be used only to deploy our infrastructure and our application in production,
    + The “dev” branch which will be used to develop the functionalities and carry out the tests.
    + Pull request in order to merge the “dev” branch on the “main” branch
  + Use of Jenkins to orchestrate all stages and set up several pipelines.
  + Use notification space on the Slack collaborative platform and email to notify us of the state of the pipeline.
  + Generation of badges to inform employees

+ DevSecOps:
  + Sonarqube analyze the code
  + Clair Image Scanner:
    + Identify vulnerabilities in built images
  + Attack generation with Gauntlt:
    + Execute attack scenarios to identify vulnerabilities in our web application :
      + xss attack
      + curl attack
      + nmap attack

### Workflow

#### Description

Continuous Delivery on the **“dev”** branch:
+ Development code update via git,
  + Continuous Integration
    + Triggering of the first pipeline thanks to the push trigger and the webhook sent to Jenkins:
    + Analysis / linter and tests of the syntax of Pyhton and Dockerfile
    + Notification on Slack and by email of the result of this pipeline

+ If the pipeline is successful, triggering and automatic execution of another pipeline thanks to the success of the first:
  + Continuous Integration
     + Analysis / linter and tests of the syntax of mardown, bash, yml files and also of the Ansible syntax
     + Configuration of the environment on the build server, then build and test our artifacts (Docker images) on the server.
     + **Security** : Vulnerability scan of builder images with Clair
     + Push our images on our  registry Dockerhub. Cleaning up the build environment.
     
  + Continuous Deployment
     + On the staging server, configuration of the environment, recovery of the necessary sources and deployment of our application in an environment close to production,
     + **Security** : Generation of attacks with several scenarios:
       + xss attack
       + curl attack (curl and xss)
       + nmap attack
     + Several tests of the proper functioning of the application (web services and database)
     + Notification on Slack and by email of the result of this pipeline

+ If the pipeline is successful, set up a Pull Request for a manager to check all the pipelines and that they are working properly.
The manager decides to accept the Pull Request and therefore merge the "dev" branch on the "main" branch to deploy the application in production.

Continuous Delivery on the **”Main”** branch:
+ A new pipeline is triggered and executed automatically after the Merge Request:
  + Analysis / linter and tests of the syntax of mardown, bash, yml files and also of the Ansible syntax
  + The build and staging steps are voluntarily forgotten,
  + On the production server, configuration of the environment, recovery of the necessary sources and deployment of our application in the production environment
  + Several tests of the proper functioning functional of the application in production (web services and database)
  + Notification on Slack and by email of the result of this pipeline.


### Technical word

Docker, docker-compose, Ansible, Tags, Playbooks, Roles, Galaxy, Jenkins, Shared-library, Pipeline, Notification, linter, DevSecOps, Clair, Gauntlt, Sonarqube

### Reference repository

+ [Source code development](https://github.com/carollebertille/phonebook-dev "Source code development")
+ [Shared-library](https://github.com/carollebertille/shared-library "Shared-library")
  
