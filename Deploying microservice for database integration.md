# Deploying microservice for database integration

In the first excerices you will deploy microservice written in Java using [Spring Cloud framework] (http://projects.spring.io/spring-cloud/)  
The microservice exposes data from the database over HTTP (translates HTTP GET request into proper SQL query and returns data)  

MySQL database was already provisioned for you and is available under 169.46.17.190. Inside there is sample inventory database that contains informations about some items

Microservices can be developed using different programing languages and can run on almost any platform. In this example we will use the microservice written in Java running in Docker container.

For you convenience git repository containing app.jar as well as coresponding Dockerfile is available [here] (https://github.com/dymaczew/micro-inventory-docker.git)

IBM Bluemix is the only public cloud that provides you with choice of 5 different target platforms. You can run your workload as:
- bare metal servers
- virtual machines
- Docker containers
- Cloud Foundry applications
- OpenWhisk event-driven workflows

To start deploying Docker containers in Bluemix you have to create a namespace in docker registry hosted on Bluemix. This namespace will identify your images and containers

  1. Login to Bluemix console
  2. Click "Catalog" in the top right part of the screen. Then select "Containers" under Apps menu on the left. Try to create any new container clicking on the name (for example "ibm-backup-restore").  

![select image] (microservices-workshop/resources/002-select-container.png)  

  3. A "Set registry namespace" dialog pops up. Provide a name of your choice and click "Save". When the namespace is created, you can go back to the dashboard and continue with the exercise

Now you will define DevOps pipeline - a process that automates building and deploying your microservice whenever source code is changed.
1. From the top left menu select "Service" and "DevOps". Then click "Create DevOps Service" button.
2. Select "Continuous Delivery" service from available services. Review payment plan (Should be 'Free') and click 'Create'.
