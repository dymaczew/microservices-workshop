# Deploying microservice for database integration

In the first excerices you will deploy microservice written in Java with [Spring Cloud framework](http://projects.spring.io/spring-cloud/)  
The microservice exposes data from the database over HTTP (translates HTTP GET request into proper SQL query and returns data)  

MySQL database was already provisioned for you and it is available under 169.46.17.190  
Inside the database, there is a sample inventory db that contains tables with informations about 'items'.

Microservices can be developed using different programing languages and can run on almost any platform. In this example we will use the microservice written in Java running in Docker container.

IBM Bluemix is the only public cloud that provides you with choice of 5 different target platforms. You can run your workload as:
- bare metal servers
- virtual machines
- Docker containers
- Cloud Foundry applications
- OpenWhisk event-driven workflows

## Examining source code
Take a look at the application source code  
1. Open the file [Application.java](https://github.com/dymaczew/cloudnative-micro-inventory-dymaczew/blob/master/src/main/java/inventory/mysql/Application.java). You will notice the following lines:  
![application-source-code](resources/005-application1-source-code.png)

The lines marked in red are responsible for initiating the Spring Cloud framework in the application, registering the microservice in Eureka registry and enabling circuit breaker (Hysterix)

2. Open the configuration file for Spring Cloud framework [here](https://github.com/dymaczew/cloudnative-micro-inventory-dymaczew/blob/master/src/main/resources/application.yml)  
The file contains externalized variables used by the framework - such as location of Eureka service or database to which application should connect.  
The values from the file can be overriden with environment variables at the runtime.

For you convenience application is already compiled and you will use git repository containing app.jar as well as corresponding Dockerfile [here](https://github.com/dymaczew/micro-inventory-docker.git)

## Deploying Docker container
To start deploying Docker containers in Bluemix you have to create a namespace in docker registry hosted on Bluemix. This namespace will identify your images and containers

  1. Login to Bluemix console
  2. Click "Catalog" in the top right part of the screen. Then select "Containers" under Apps menu on the left. Try to create any new container clicking on the name (for example "ibm-backup-restore").  

![select image](resources/002-select-container.png)  

  3. A "Set registry namespace" dialog pops up. Provide a name of your choice and click "Save". When the namespace is created, you can go back to the dashboard and continue with the exercise

### Setting up Continuous Delivery service

Now you will define DevOps service - a process that automates building and deploying your microservices whenever source code is changed.
1. From the top left menu select "Service" and "DevOps". Then click "Create DevOps Service" button.
2. Select "Continuous Delivery" service from available services. Review payment plan (Should be 'Free') and click 'Create'.
3. On the "Get started with Continuous Delivery" select "Start from a toolchain template". Scroll down and then select "Build your own toolchain" in "Other toolchains" category. Provide the name for the toolchain

### Adding GitHub integration to the toolchain
4. Click on "Add a Tool"
5. Select github from available pallette. 
![github](resources/003-select-github-integration.png)  
6. If you haven't done it before you have to connect Bluemix wit Github. Provide the name and password for your github account. Click on the "Authorize" button and refresh the page
7. On "Configure the Integration" screen select  
    Repository type: Clone.  
    Provide new  repository name, for example `micro-inventory-\<username\>`.  
    Provice the source path https://github.com/dymaczew/micro-inventory-docker.git  
![micro-inventory-docker](resources/004-details-of-github-integration.png)
8. Click `Create integration`

### Adding Delivery Pipeline to the toolchain

1. As a second tool in your toolchain you will add 
    
