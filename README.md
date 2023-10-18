
# DevOps 101

This readme corresponds to  for introduction to DevOps.
Follow this tutorial to deploy an angular application to the AWS EC2.



## Roadmap

- Write dockerfile

- Build and upload image to docker hub

- Create and setup EC2 instance on AWS

- Clean up of resources


## Write dockerfile
Dockerfile can be placed in any repository or any folder inside your repository. For this tutorial, create the dockerfile right beside the package.json file. 

```
FROM --platform=linux/amd64 node:20-alpine as build
WORKDIR /crud-app
COPY package.json /crud-app/
RUN npm install
COPY . /crud-app/
RUN npm run build --prod
# CMD ["npm", "run", "start"]


FROM --platform=linux/amd64 nginx:alpine as nginx
COPY --from=build /crud-app/dist/angular-app /usr/share/nginx/html
```

## Build and upload image to docker hub

- Signup and login to [Docker Hub](https://hub.docker.com/)
- Run the following commands to build and push the image to docker hub
- Open terminal/CLI on your machine and type in below command to login to docker hub. Enter username and password when prompted.
    ```
    docker login
    ```
- Build docker image
    ```
    docker build -t <username>/<image_name>:<tag> .
    ```
    In our case, the command will look like
    ```
    docker build -t roshada/angular-crud:latest .
    ```
- Push docker image to hub
    ```
    docker push <username>/<image_name>:<tag>
    ```
    In our case, the command will look like
    ```
    docker push roshada/angular-crud:latest
    ```

## Create and setup EC2 instance on AWS
Signup at [AWS](https://aws.amazon.com/free/) for your own free account and credit eligibility. Head over to search bar and type EC2 instances. 

A new window will appear with EC2 creation configurations. Just enter a custom name and leave the rest of the configurations as default for this tutorial.

#### Set up docker 
Update the linux system
```
sudo yum update -y
```
Install the most recent Docker Community Edition package
```
sudo yum install docker
```
Start docker
```
sudo systemctl start docker
```
Check if docker is actually running
```
sudo service docker status
```
Add ec2-user to docker group
```
sudo usermod -a -G docker ec2-user
newgrp docker
```
Check docker version
```
docker --version
```
Run below commands in the VM to pull and run the docker image
```
docker login
```
```
docker pull roshada/angular-crud:latest
```
```
docker run -it -p 80:80n -it -p 80:80 roshada/angular-crud:latest
```
Now lastly, open the port on which the VM is running.  
In our case, the port is 80.  
Go to the securirty group and enable port 80 from 0.0.0.0/0. 
Now go to AWS console and copy the EC2 instance's Public IPv4 DNS.



Put the DNS in the browser and voila!!! We are done!!!  
Congratulation and happy learning!  
**PS: Don't forget to clean up the resources.**
