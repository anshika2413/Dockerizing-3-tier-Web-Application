# Flow Diagram
![image](https://github.com/user-attachments/assets/4ec5dbf2-fb9d-498a-a35a-2b5e60c92e5f)

Step-1: Create a database in AWS RDS

Step-2: Create a container to connect to database in the AWS RDS and upload data. 

Step 3: Building Backend Docker image

Step 4: Building Frontend Docker image

Step 5: Running FE and BE containers out of docker images.

Step 6: Run localhost in browser and browse the E-commerce app deployed.

# To Create a Database in RDS
– login to AWS

– Services >> Database >> RDS >> Create database.

– Choose a database creation method >> Standard create.

– Engine options >> MySql

– Templates >> Free Tier

– Settings >> Credentials Settings >> Give password.

– Instance configuration >> db.t3.micro

– Connectivity >> Don’t connect to an EC2.

– Public access >> Yes

– VPC security group (firewall) >> create new.

– Database authentication >> Password authentication and create.

# Update Security Group of the database
Navigate to Databases and select security-group the one that has been created.

Connectivity & security >> VPC security groups >> Edit inbound rules >> PORT 3306 (MYSQL) and Source -Anywhere – save

# Create a container to connect to database in the AWS RDS and upload data.
Follow below steps :

Grab database Endpoint URL from the AWS RDS
To grab database endpoint – browse to AWS-RDS-database >> Connectivity & security >> endpoint and port.

Create a container and connect to it from local machine
The below command needs to be run on a local PC (in my case it’s a Windows cmd). This will create a container from docker image node:14-alpine and also login to the container.

                                                  winpty docker run -it -p 3306:3306 node:14-alpine sh
Once logged in to the container, follow below steps in which we create a directory, clone the git repo, connect to the database server and create database and then upload db dump to the database.
 
    Create directory :
mkdir -p /home/webapp
cd /home/webapp

    Install mysql and git in the container
apk add git mysql mysql-client

    Git clone 
git clone https://github.com/devopsenlight/web-app-deployment-on-cloud.git
cd /home/webapp/web-app-deployment-on-cloud/backend

    Connect to the database server and create database 
mysql -h test-databaseq.caxdt0nmodrh.ap-south-1.rds.amazonaws.com -u admin -p
create database webapp;
exit;

    Upload db dump to the database 
mysql -h test-databaseq.caxdt0nmodrh.ap-south-1.rds.amazonaws.com -u admin -p webapp < sql_dump.sql

# To build docker image using Dockerfile
    docker build . --no-cache -t webapp-be (For Backend)
    docker build . --no-cache -t webapp-fe (For Frontend)

# Running Frontend and Backend containers out of images created
    docker run -it -d -p 5000:5000 -p 3306:3306 webapp-be
    docker run -it -d -p 80:80 webapp-fe

Thats all, database is available in RDS, backend container is connected to database using port 3306 and backend container is connected to frontend container via port 5000 and frontend container is running and accessible via port 80.


