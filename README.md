## 🎓🔥 STARGATE, a gateway for multi models data API 🔥🎓

[![License Apache2](https://img.shields.io/hexpm/l/plug.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Discord](https://img.shields.io/discord/685554030159593522)](https://discord.com/widget?id=685554030159593522&theme=dark)

![image](pics/splash.png?raw=true)

# Table of content

1. [Prerequisites](#1-prerequisite-install-docker-and-docker-compose)
  
2. Start the Demo

3. Use CQL API

3. Use REST API

4. Use Document API

5. Use GraphQL API


## 1. Prerequisite, Install docker and docker-compose ##

To run the demo you need `docker` and `docker-compose`. Skip this steps if you have them available already

### 1. Install Docker

Docker is an open-source project that automates the deployment of software applications inside containers by providing an additional layer of abstraction and automation of OS-level virtualization on Linux.

![Windows](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/windows32.png?raw=true) : To install on **windows** please use the following installer [Docker Desktop for Windows Installer](https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe)

![osx](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/mac32.png?raw=true) : To install on **MAC OS**  use [Docker Desktop for MAC Installer](https://download.docker.com/mac/stable/Docker.dmg) or [homebrew](https://docs.brew.sh/Installation) with following commands:
```bash
# Fetch latest version of homebrew and formula.
brew update              
# Tap the Caskroom/Cask repository from Github using HTTPS.
brew tap caskroom/cask                
# Searches all known Casks for a partial or exact match.
brew search docker                    
# Displays information about the given Cask
brew cask info docker
# Install the given cask.
brew cask install docker              
# Remove any older versions from the cellar.
brew cleanup
# Validate installation
docker -v
```

![linux](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/linux32.png?raw=true) : To install on linux (centOS) you can use the following commands
```bash
# Remove if already install
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# Utils
sudo yum install -y yum-utils

# Add docker-ce repo
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
# Install
sudo dnf -y  install docker-ce --nobest
# Enable service
sudo systemctl enable --now docker
# Get Status
systemctl status  docker

# Logout....Lougin
exit
# Create user
sudo usermod -aG docker $USER
newgrp docker

# Validation
docker images
docker run hello-world
docker -v
```
[🏠 Back to Table of Contents](#Table-Of-content)

### 2. Install Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. It uses YAML files to configure the application's services and performs the creation and start-up process of all the containers with a single command. The `docker-compose` CLI utility allows users to run commands on multiple containers at once, for example, building images, scaling containers, running containers that were stopped, and more. Please refer to [Reference Documentation](https://docs.docker.com/compose/install/) if you have for more detailed instructions.

![Windows](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/windows32.png?raw=true) : Already **included** in the previous package *Docker for Windows*

![osx](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/mac32.png?raw=true) : Already **included** in the previous package *Docker for Mac*

![linux](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/linux32.png?raw=true) : To install on linux (centOS) you can use the following commands

```bash
# Download deliverable and move to target location
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Allow execution
sudo chmod +x /usr/local/bin/docker-compose
```
[🏠 Back to Table of Contents](#exercises)

## 2. Start the demo

**✅ Clone the repository** :

Download the repository as a zip [here] or clone with the following git command

```
git clone https://github.com/datastaxdevs/conference-2020-dataconla-stargate.git
``` 

**✅ Start all containers** :

We provide a `docker-compose.yaml` file ready to go with a `Cassandra 3.11.8` backend and stargate in version `0.0.8`

To start use the following

```
docker-compose up -d
```

**👁️ Expected output**

```
Starting backend-1 ... done
Starting stargate  ... done
Starting backend-2 ... done
Starting backend-3 ... done
```

Wait for all services are up. You need the 4 containers. 

You may have to relaunch the command multiple times as stargate need the 3 nodes to be up to join the cluster.

```bash
docker ps
```

**👁️ Expected output**

```bash
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                                                       NAMES
237d9137884f        stargateio/stargate-3_11:v0.0.8   "./starctl"              25 hours ago        Up 4 hours          0.0.0.0:8080-8082->8080-8082/tcp, 0.0.0.0:9045->9042/tcp    stargate
b3a10c48dccc        cassandra:3.11.8                  "docker-entrypoint.s…"   25 hours ago        Up 4 hours          7000-7001/tcp, 7199/tcp, 9042/tcp, 9160/tcp                 backend-3
68be3b9bfc23        cassandra:3.11.8                  "docker-entrypoint.s…"   25 hours ago        Up 4 hours          7000-7001/tcp, 7199/tcp, 9042/tcp, 9160/tcp                 backend-2
23b009a08872        cassandra:3.11.8                  "docker-entrypoint.s…"   25 hours ago        Up 4 hours          7000-7001/tcp, 7199/tcp, 9160/tcp, 0.0.0.0:9044->9042/tcp   backend-1
```

- You should be able to access the GRAPH QL PORTAL on [http://localhost:8080/playground](http://localhost:8080/playground)

![image](pics/playground-home.png?raw=true)


- You should be able to access the Swagger UI on [http://localhost:8082/swagger-ui/#/](http://localhost:8082/swagger-ui/#/)

![image](pics/swagger-home.png?raw=true)



## 3. DEMO !

```
docker exec -it `docker ps | grep backend-1 | cut -b 1-12` nodetool status
```
Expected output:

```bash
nodetool status
Datacenter: datacenter1
=======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address     Load       Tokens       Owns (effective)  Host ID                               Rack
UN  172.20.0.4  179.98 KiB  8            1.1%              9873d76b-b439-4c81-92d7-4de103052af0  rack1
UN  172.20.0.5  370.43 KiB  256          35.0%             52e259b8-92ee-402c-b3b2-b1b39ad10ebb  rack1
UN  172.20.0.2  354.2 KiB  256          34.8%             9dc3b120-19a7-4eda-8828-83532d816fcc  rack1
UN  172.20.0.3  341.41 KiB  256          29.1%             b385cbc3-7d30-46d3-bd14-f1b8ee01044d  rack1
```

Get Stargate IP
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' stargate
```

```


### 1. REST API

- Generate an auth token

```bash
curl -L -X POST 'http://localhost:8081/v1/auth' \
  -H 'Content-Type: application/json' \
  --data-raw '{
    "username": "cassandra",
    "password": "cassandra"
}'
```

Expected output 
```
{"authToken":"74be42ef-3431-4193-b1c1-cd8bd9f48132"}
```

- Using Swagger List keyspaces 







### 1. Start the database

**✅ Create an free-forever Cassandra database with DataStax Astra**: [click here to get started](https://astra.datastax.com/register?utm_source=github&utm_medium=referral&utm_campaign=spring-petclinic-reactive) 🚀


![Astra Registration Screen](doc/img/db-auth.png?raw=true)


**✅ Use the form to create new database**

On the Astra home page locate the **Add Database** button

![Astra Database Creation Form](doc/img/db-creation-1.png?raw=true)

Select the **free tier** plan, this is a true free tier, free forever and no payment method asked 🎉 🎉

![Astra Database Creation Form](doc/img/db-creation-2.png?raw=true)

Select the proper region and click the `configure` button. The number of regions and cloud providers are limited in the free tier but please notice you can run the DB on any cloud with any VPC Peering.

![Astra Database Creation Form](doc/img/db-creation-3.png?raw=true)

Fill the `database name`, `keyspace name`, `username` and `password`. *Please remember your password as you will be asked to provide it when application start the first time.*

![Astra Database Creation Form](doc/img/db-creation-4.png?raw=true)

**✅ View your Database and connect**

View your database. It may take 2-3 minutes for your database to spin up. You will receive an email at that point.

**👁️ Expected output**

*Initializing*

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/dashboard-pending-1000.png?raw=true)

Once the database is ready, notice how the status changes from `pending` to `Active` and Astra enables the **connect** button.

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/dashboard-withdb-1000.png?raw=true)

### 2. Copy credentials to connect

**✅ Navigate to your credentials**

Locate the combo `Organization: <Your email>` on the top navigation. On the right side of your organization, click the elipsis (...) then click your `<Your email>`.

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/organization-combo-1000.png?raw=true)

You should land to the following screen. Scroll down to the bottom of the page to locate the `Service Account` in `Security Settings`

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/organization-home-1000.png?raw=true)

**✅ Create Service Account**

Create a service account by clicking `Add Service Account` button above the section as shown below

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/security-settings-annotated.png?raw=true)

When panel open on the right, click `Add` 

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/security-add-org-annotated.png?raw=true)

**✅ Copy credentials to your clipboard**

Click the ellipsis at end of Service Account row to open menu as select `Copy Credentials`

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/organization-copycredentials-1000.png?raw=true)

The credentials you copied to the clipboard look like the following JSON, we will use it in gitpod to enable connectivity.
```json
{
  "clientId":"149de2c7-9b07-41b3-91ad-9453dee4dc54",
  "clientName":"cedrick.lunven@datastax.com",
  "clientSecret":"aaaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa"
}
```

### 3. Start in Gitpod

**✅ Open Gitpod (with creds copied to clipboard)**

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/Datastax-Academy/workshop-spring-reactive)

When you first launch gitpod, it builds the image.
![image](doc/img/building-workspace.png?raw=true)

Git pod pulls the image.
![image](doc/img/pulling-image.png?raw=true)



**✅ Paste credentials in Gitpod terminal**

Once your Gitpod loads the workspace, you'll be asked to paste your service account credentials in the Gitpod terminal at the bottom of the screen. The [setup.sh](setup.sh) script at the root of the repository is what asks this question.

![image](doc/img/script-copy-creds.png?raw=true)

**✅ Open Swagger UI in browser**

When gitpod finishes building the app, a new tab will open in your browser showing the following.

![image](doc/img/exec-start.png?raw=true)

**🎉 Celebrate!**

You've successfully built the Spring Petclinic Reactive backend application!

![image](doc/img/exec-api-page.png?raw=true)

**✅ Start the Web UI** :

You may have noticed another terminal named `spring-petclinic-angular`. This is where the UI should start.

![image](doc/img/start-ui.png?raw=true)

After answering the question about analytics usage, you should be able to access the UI on a new tab.








