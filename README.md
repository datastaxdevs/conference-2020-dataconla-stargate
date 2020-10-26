## 🎓🔥 STARGATE, a gateway for multi models data API 🔥🎓

[![License Apache2](https://img.shields.io/hexpm/l/plug.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Discord](https://img.shields.io/discord/685554030159593522)](https://discord.com/widget?id=685554030159593522&theme=dark)

![image](pics/splash.png?raw=true)

## Table of content

1. [Prerequisites](#1-prerequisite-install-docker-and-docker-compose)
2. [Start the Demo](#2-start-the-demo)
3. [Use CQL API](#3-use-cql-api)
4. [Use REST API](#4-use-rest-api-swagger)
5. [Use Document API](#5-use-document-api-swagger)
6. [Use GraphQL API](#6-use-graphql-api-portal)
7. [Create an Astra Instance](#7-create-your-astra-instance)

## 1. Prerequisite, Install docker and docker-compose ##

To run the demo you need `docker` and `docker-compose`. Skip this steps if you have them available already

### 1.1 Install Docker

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

### 1.2 Install Docker Compose

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

### 1.3 Install curl

[Reference Article](https://help.ubidots.com/en/articles/2165289-learn-how-to-install-run-curl-on-windows-macosx-linux)

![Windows](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/windows32.png?raw=true) : Already **included** as stated [here](https://www.thewindowsclub.com/how-to-install-curl-on-windows-10)

![osx](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/mac32.png?raw=true) : Use brew

```
brew install curl
```

![linux](https://github.com/DataStax-Academy/kubernetes-workshop-online/blob/master/4-materials/images/linux32.png?raw=true) :Use your package installer like `yum` or apt-get

```
sudo apt-get install curl
```

[🏠 Back to Table of Contents](#table-of-content)


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

**👁️ Expected output**
![image](pics/playground-home.png?raw=true)


- You should be able to access the Swagger UI on [http://localhost:8082/swagger-ui/#/](http://localhost:8082/swagger-ui/#/)

**👁️ Expected output**
![image](pics/swagger-home.png?raw=true)

[🏠 Back to Table of Contents](#table-of-content)

## 3. Use CQL API

**✅ Check that everything is OK** :

```
docker exec -it `docker ps | grep backend-1 | cut -b 1-12` nodetool status
```

**👁️ Expected output**
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

**✅ Get Stargate IP** :
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' stargate
```

**👁️ Expected output**
```
172.20.0.4
```

**✅ Start CQLSH** :

- Use this IP to connect with a cqlsh. *Note that the stargate image itself does not provide it we use the cqlsh from backend-1 as a sample client.*

```
docker exec -it `docker ps | grep backend-1 | cut -b 1-12` cqlsh 172.20.0.4
```

**✅ Create data model** :

```sql
CREATE KEYSPACE IF NOT EXISTS keyspace1
  WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3'}
  AND durable_writes = true;
```

- Create schema a UDT and a table

```sql
use keyspace1;

CREATE TYPE IF NOT EXISTS video_format (
  width   int,
  height  int
);

CREATE TABLE IF NOT EXISTS videos (
 videoid   uuid,
 title     text,
 upload    timestamp,
 email     text,
 url       text,
 tags      set <text>,
 frames    list<int>,
 formats   map <text,frozen<video_format>>,
 PRIMARY KEY (videoid)
);

describe keyspace;
```

**✅ Use the data model** :

- Insert value using plain CQL

```sql
INSERT INTO videos(videoid, email, title, upload, url, tags, frames, formats)
VALUES(uuid(), 'clu@sample.com', 'sample video', 
     toTimeStamp(now()), 'http://google.fr',
     { 'cassandra','accelerate','2020'},
     [ 1, 2, 3, 4], 
     { 'mp4':{width:1,height:1},'ogg':{width:1,height:1}});
```

- Insert Value using JSON

```sql
INSERT INTO videos JSON '{
   "videoid":"e466f561-4ea4-4eb7-8dcc-126e0fbfd573",
     "email":"clunven@sample.com",
     "title":"A Second videos",
     "upload":"2020-02-26 15:09:22 +00:00",
     "url": "http://google.fr",
     "frames": [1,2,3,4],
     "tags":   [ "cassandra","accelerate", "2020"],
     "formats": { 
        "mp4": {"width":1,"height":1},
        "ogg": {"width":1,"height":1}
     }
}';
```

- Read values

```sql
select * from videos;
```

- Read by id
```sql
select * from videos where videoid=e466f561-4ea4-4eb7-8dcc-126e0fbfd573;
```

- Quit `cqlsh`

```sql
exit
```

[🏠 Back to Table of Contents](#table-of-content)

## 4. Use REST API (swagger)

This walkthrough has been realized using the [REST API Quick Start](https://stargate.io/docs/stargate/0.1/quickstart/quick_start-rest.html)


**✅  Generate an auth token** :

```bash
curl -L -X POST 'http://localhost:8081/v1/auth' \
  -H 'Content-Type: application/json' \
  --data-raw '{
    "username": "cassandra",
    "password": "cassandra"
}'
```

Copy the token value (here `74be42ef-3431-4193-b1c1-cd8bd9f48132`) in your clipboard.

**👁️ Expected output**
```
{"authToken":"74be42ef-3431-4193-b1c1-cd8bd9f48132"}
```

**✅ List keyspaces** : 

Locate the `SCHEMAS` part of the API

![image](pics/swagger-general.png?raw=true)

 Localt `listAllKeyspaces` [method on GET](http://localhost:8082/swagger-ui/#/schemas/listAllKeyspaces)

![image](pics/swagger-list-keyspace.png?raw=true)

- Click `Try it out`
- Provide your token in the field `X-Cassandra-Token`
- Click on `Execute`

**✅ Creating a keyspace2** : 

- [createKeyspace](http://localhost:8082/swagger-ui/#/schemas/createKeyspace)
- Data
```json
{"name": "keyspace2","replicas": 3}
```

**✅ Creating a Table** : 

- [addTable](http://localhost:8082/swagger-ui/#/schemas/addTable)
- X-Cassandra-Token: `<your_token>`
- keyspace: `keyspace2`
- Data
```json
{
  "name": "users",
  "columnDefinitions":
    [
        {
        "name": "firstname",
        "typeDefinition": "text"
      },
        {
        "name": "lastname",
        "typeDefinition": "text"
      },
      {
        "name": "email",
        "typeDefinition": "text"
      },
        {
        "name": "favorite color",
        "typeDefinition": "text"
      }
    ],
  "primaryKey":
    {
      "partitionKey": ["firstname"],
      "clusteringKey": ["lastname"]
    },
  "tableOptions":
    {
      "defaultTimeToLive": 0,
      "clusteringExpression":
        [{ "column": "lastname", "order": "ASC" }]
    }
}
```

Now Locate the `DATA` part of the API

**✅ Insert a row** : 

- [createRow](http://localhost:8082/swagger-ui/#/data/createRow)
- X-Cassandra-Token: `<your_token>`
- keyspace: `keyspace2`
- table: `users`
- Data
```json
{   
    "firstname": "Mookie",
    "lastname": "Betts",
    "email": "mookie.betts@gmail.com",
    "favorite color": "blue"
}
```

```json
{
    "firstname": "Janesha",
    "lastname": "Doesha",
    "email": "janesha.doesha@gmail.com",
    "favorite color": "grey"
}
```

**✅ Read data** : 

- [getAllRows](http://localhost:8082/swagger-ui/#/data/getAllRows)
- X-Cassandra-Token: `<your_token>`
- keyspace: `keyspace2`
- table: `users`


**✅ Update a row** : 

You can do them in curl

```
export AUTH_TOKEN=<your_token>
```

```
curl --location \
--request PUT 'localhost:8082/v2/keyspaces/users_keyspace/users/Mookie/Betts' \
--header "X-Cassandra-Token: $AUTH_TOKEN" \
--header 'Content-Type: application/json' \
--data '{
    "email": "mookie.betts.new-email@email.com"
}'
```

**✅ Delete a row** : 
```
curl --location \
--request DELETE 'localhost:8082/v2/keyspaces/users_keyspace/users/Mookie' \
--header "X-Cassandra-Token: $AUTH_TOKEN" \
--header 'Content-Type: application/json'
```

[🏠 Back to Table of Contents](#table-of-content)

## 5. Use Document API (swagger+curl)

This walkthrough has been realized using the [Quick Start](https://stargate.io/docs/stargate/0.1/quickstart/quick_start-document.html)

**✅ Generate an auth token** :

Same as Rest API generate a `auth token` 
```bash
curl -L -X POST 'http://localhost:8081/v1/auth' \
  -H 'Content-Type: application/json' \
  --data-raw '{
    "username": "cassandra",
    "password": "cassandra"
}'
```

Save output as an environment variable

```
export AUTH_TOKEN=5d746e40-97cf-490b-ab0d-68cfbc5d2ef3
```

**✅ Creating a namespace** :

- Access [createNamespace](http://localhost:8082/swagger-ui/#/documents/createNamespace) in swagger UI
- Fill with Header `X-Cassandra-Token` with `<your_token>`
- Use this payload as JSON
```json
{ "name": "namespace1", "replicas": 3 }
```

**✅ Checking namespace existence** :

- Access [getAllNamespaces](http://localhost:8082/swagger-ui/#/documents/getAllNamespaces) in swagger UI
- Fill with Header `X-Cassandra-Token` with `<your_token>`
- For `raw` you can use either `true` or `false`

**👁️ Expected output**
```json
{
  "data": [
    { "name": "system_distributed" },
    { "name": "system" },
    { "name": "data_endpoint_auth"},
    { "name": "keyspace1" },
    { "name": "namespace1"},
    { "name": "system_schema"},
    { "name": "keyspace2" },
    { "name": "stargate_system"},
    { "name": "system_auth" },
    { "name": "system_traces"}
  ]
}
```

**✅ Create a document** :

*Note: operations requiring providing `namespace` and `collections` on the swagger UI seems not functional. We are switching to CURL the API is working, this is a documentation bug that has been notified to the development team.*

```bash
curl --location \
--request POST 'localhost:8082/v2/namespaces/namespace1/collections/videos' \
--header "X-Cassandra-Token: $AUTH_TOKEN" \
--header 'Content-Type: application/json' \
--data '{
   "videoid":"e466f561-4ea4-4eb7-8dcc-126e0fbfd573",
     "email":"clunven@sample.com",
     "title":"A Second videos",
     "upload":"2020-02-26 15:09:22 +00:00",
     "url": "http://google.fr",
     "frames": [1,2,3,4],
     "tags":   [ "cassandra","accelerate", "2020"],
     "formats": { 
        "mp4": {"width":1,"height":1},
        "ogg": {"width":1,"height":1}
     }
}'
```

**👁️ Expected output**:
```json
{
  "documentId":"5d746e40-97cf-490b-ab0d-68cfbc5d2ef3"
}
```

**✅ Retrieve documents** :

```bash
curl --location \
--request GET 'localhost:8082/v2/namespaces/namespace1/collections/videos?page-size=3' \
--header "X-Cassandra-Token: $AUTH_TOKEN" \
--header 'Content-Type: application/json'
```

**👁️ Expected output**:
```json
{
  "data":{
    "5d746e40-97cf-490b-ab0d-68cfbc5d2ef3":{
      "email":"clunven@sample.com",
      "formats":{"mp4":{"height":1,"width":1},"ogg":{"height":1,"width":1}},"frames":[1,2,3,4],
      "tags":["cassandra","accelerate","2020"],"title":"A Second videos","upload":"2020-02-26 15:09:22 +00:00","url":"http://google.fr","videoid":"e466f561-4ea4-4eb7-8dcc-126e0fbfd573"
     }
   }
}
```

**✅ Retrieve 1 document** :

```bash
curl -L \
-X GET 'localhost:8082/v2/namespaces/namespace1/collections/videos/5d746e40-97cf-490b-ab0d-68cfbc5d2ef3' \
--header "X-Cassandra-Token: $AUTH_TOKEN" \
--header 'Content-Type: application/json'
```

**👁️ Expected output**:
```json
{
  "documentId":"5d746e40-97cf-490b-ab0d-68cfbc5d2ef3",
  "data":{
     "email":"clunven@sample.com",
     "formats":{"mp4":{"height":1,"width":1},"ogg":{"height":1,"width":1}},
     "frames":[1,2,3,4],
     "tags":["cassandra","accelerate","2020"],
     "title":"A Second videos",
     "upload":"2020-02-26 15:09:22 +00:00",
     "url":"http://google.fr",
     "videoid":"e466f561-4ea4-4eb7-8dcc-126e0fbfd573"
   }
}
```

**✅ Search for document by properties** :

```bash
curl -L -X  GET 'localhost:8082/v2/namespaces/namespace1/collections/videos?where=\{"email":\{"$eq":"clunven@sample.com"\}\}' \
--header "X-Cassandra-Token: $AUTH_TOKEN" \
--header 'Content-Type: application/json'
```

**👁️ Expected output**:
```json
{"data":{
   "5d746e40-97cf-490b-ab0d-68cfbc5d2ef3":{
      "email":"clunven@sample.com",
      "formats":{"mp4":{"height":1,"width":1},"ogg":{"height":1,"width":1}},
      "frames":[1,2,3,4],
      "tags":["cassandra","accelerate","2020"],
      "title":"A Second videos",
      "upload":"2020-02-26 15:09:22 +00:00",
      "url":"http://google.fr",
      "videoid":"e466f561-4ea4-4eb7-8dcc-126e0fbfd573"
    }
  }
}
```

[🏠 Back to Table of Contents](#table-of-content)

## 6. Use GraphQL API (portal)

This walkthrough has been realized using the [GraphQL Quick Start](https://stargate.io/docs/stargate/0.1/quickstart/quick_start-graphql.html)

Same as Rest API generate a `auth token` 

**✅ Generate Auth token** :
```bash
curl -L -X POST 'http://localhost:8081/v1/auth' \
  -H 'Content-Type: application/json' \
  --data-raw '{
    "username": "cassandra",
    "password": "cassandra"
}'
```

Save output as an environment variable
```
export AUTH_TOKEN=7c37bda5-7360-4d39-96bc-9765db5773bc
```

**✅ Open GraphQL Playground** :

- You should be able to access the GRAPH QL PORTAL on [http://localhost:8080/playground](http://localhost:8080/playground)

You can check on the right of the playground that you have access to documentation and schema which is the neat part about graphQL

**👁️ Expected output**
![image](pics/playground-home.png?raw=true)

**✅ Creating a keyspace** :

Before you can start using the GraphQL API, you must first create a Cassandra keyspace and at least one table in your database. If you are connecting to a Cassandra database with existing schema, you can skip this step.

Inside the GraphQL playground, navigate to http://localhost:8080/graphql-schema and create a keyspace by executing the following mutation:

```
mutation createKeyspaceLibrary {
  createKeyspace(name:"library", replicas: 1)
}
```

Add the auth token to the HTTP Headers box in the lower lefthand corner:
```
{
  "x-cassandra-token":"7c37bda5-7360-4d39-96bc-9765db5773bc"
}
```

**👁️ Expected output**
![image](pics/graphql-createkeyspace.png?raw=true)

**✅ Creating a Table** :

- Use this query
```
mutation {
  books: createTable(
    keyspaceName:"library",
    tableName:"books",
    partitionKeys: [ # The keys required to access your data
      { name: "title", type: {basic: TEXT} }
    ]
    values: [ # The values associated with the keys
      { name: "author", type: {basic: TEXT} }
    ]
  )
  authors: createTable(
    keyspaceName:"library",
    tableName:"authors",
    partitionKeys: [
      { name: "name", type: {basic: TEXT} }
    ]
    clusteringKeys: [ # Secondary key used to access values within the partition
      { name: "title", type: {basic: TEXT}, order: "ASC" }
    ]
  )
}
```

**👁️ Expected output**
![image](pics/graphql-createtables.png?raw=true)

**✅ Populating Table** :

Any of the created APIs can be used to interact with the GraphQL data, to write or read data.

First, let’s navigate to your new keyspace `library` inside the playground. Change tab to `graphql` and pick url `/graphql/library`.

- Use this query
```
mutation {
  moby: insertBooks(value: {title:"Moby Dick", author:"Herman Melville"}) {
    value {
      title
    }
  }
  catch22: insertBooks(value: {title:"Catch-22", author:"Joseph Heller"}) {
    value {
      title
    }
  }
}
```

- Don't forget to update the header again
```
{
  "x-cassandra-token":"7c37bda5-7360-4d39-96bc-9765db5773bc"
}
```
**👁️ Expected output**
![image](pics/graphql-insertdata.png?raw=true)


**✅ Read data** :

Stay on the same screen and sinmply update the query with 
```
query oneBook {
    books (value: {title:"Moby Dick"}) {
      values {
        title
        author
      }
    }
}
```

**👁️ Expected output**
![image](pics/graphql-readdata.png?raw=true)


[🏠 Back to Table of Contents](#table-of-content)

## 7. Create your ASTRA Instance

**✅ Create an free-forever Cassandra database with DataStax Astra**: [click here to get started](https://astra.datastax.com/register?utm_source=github&utm_medium=referral&utm_campaign=spring-petclinic-reactive) 🚀


![Astra Registration Screen](pics/db-auth.png?raw=true)


**✅ Use the form to create new database**

On the Astra home page locate the **Add Database** button

![Astra Database Creation Form](pics/db-creation-1.png?raw=true)

Select the **free tier** plan, this is a true free tier, free forever and no payment method asked 🎉 🎉

![Astra Database Creation Form](pics/db-creation-2.png?raw=true)

Select the proper region and click the `configure` button. The number of regions and cloud providers are limited in the free tier but please notice you can run the DB on any cloud with any VPC Peering.

![Astra Database Creation Form](pics/db-creation-3.png?raw=true)

Fill the `database name`, `keyspace name`, `username` and `password`. *Please remember your password as you will be asked to provide it when application start the first time.*

![Astra Database Creation Form](pics/db-creation-4.png?raw=true)

**✅ View your Database and connect**

View your database. It may take 2-3 minutes for your database to spin up. You will receive an email at that point.

**👁️ Expected output**

*Initializing*

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/dashboard-pending-1000.png?raw=true)

Once the database is ready, notice how the status changes from `pending` to `Active` and Astra enables the **connect** button.

![my-pic](https://github.com/datastaxdevs/shared-assets/blob/master/astra/dashboard-withdb-1000.png?raw=true)

