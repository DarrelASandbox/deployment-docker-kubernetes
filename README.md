<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-project">About The Project</a></li>
    <li><a href="#docker-basics">Docker Basics</a></li>
    <li><a href="#images--containers">Images & Containers</a></li>
    <li><a href="#data--volumes">Data & Volumes</a></li>
    <li><a href="#networking">Networking</a></li>
    <li><a href="#multi-containers-app">Multi-Containers App</a></li>
    <li><a href="#docker-compose">Docker Compose</a></li>
    <li><a href="#utility-containers">Utility Containers</a></li>
    <li><a href="#laravel--php">Laravel & PHP</a></li>
    <li><a href="#deployment">Deployment</a></li>
    <li><a href="#aws-ec2">AWS EC2</a></li>
    <li><a href="#aws-ecs">AWS ECS</a></li>
    <li><a href="#kubernetes-basics">Kubernetes Basics</a>
      <ol>
        <li><a href="#pod-object">Pod Object</a></li>
        <li><a href="#deployment-object">Deployment Object</a></li>
        <li><a href="#service-object">Service Object</a></li>
      </ol>
    </li>
    <li><a href="#kubernetes-data--volumes">Kubernetes Data & Volumes</a></li>
    <li><a href="#kubernetes-networking">Kubernetes Networking</a></li>
    <li><a href="#kubernetes-deployment">Kubernetes Deployment</a>
      <ol>
        <li><a href="#aws-eks">AWS EKS</a></li>
        <li><a href="#adding-efs-as-a-volume-with-the-csi-volume-type">Adding EFS as a Volume (with the CSI Volume Type)</a></li>
      </ol>
    </li>
  </ol>
</details>

&nbsp;

## About The Project

- Docker & Kubernetes: The Practical Guide [2022 Edition]
- Learn Docker, Docker Compose, Multi-Container Projects, Deployment and all about Kubernetes from the ground up!
- [Maximilian Schwarzmüller](https://github.com/maxschwarzmueller)
- [Academind](https://academind.com/)

&nbsp;

## Docker Basics

- [Docker](https://www.docker.com/) is a container technology: A tool for creating and managing containers.
  - <b>Environment: </b> The runtimes, languages & frameworks
  - Development environment and production environment are often not the same
- <b>Virtual Machines</b>

|                               Pro                                |                                     Con                                     |
| :--------------------------------------------------------------: | :-------------------------------------------------------------------------: |
|                      Separated environments                      |                    Redundant duplication, waste of space                    |
|         Environment-specific configurations are possible         |               Performance can be slow, boot times can be long               |
| Environment configurations can be shared and reproduced reliably | Reproducing on another computer/ server is possible but may still be tricky |

|                     Docker Containers                      |                        Virtual Machines                         |
| :--------------------------------------------------------: | :-------------------------------------------------------------: |
|   Low impact on OS, very fast, minimal disk space usage    |      Bigger impact on OS, slower, higher disk space usage       |
|       Sharing, re-building and distribution is easy        |    Sharing, re-building and distribution can be challenging     |
| Encapsulate apps/ environments instead of "whole machines" | Encapsulate "whole machines" instead of just apps/ environments |

- <b>Docker Tools & Building blocks</b>:
  - Docker Engine
  - Docker Desktop (incl. Daemon & CLI)
  - Docker Hub
  - Docker Compose

![docker-core-concepts](diagrams/docker-core-concepts.png)

- Foundation

  - Images & Containers
  - Data & Volumes (in Containers)
  - Containers & Networking

- "Real Life"
  - Multi-Container Projects
  - Using Docker-Compose
  - "Utility Containers"
  - Deploying Docker Containers
- Kubernetes
  - Basics
  - Data & Volumes
  - Networking
  - Deploying a Kubernetes Clusters

&nbsp;

---

&nbsp;

## Images & Containers

|                  Images                  |                      Containers                       |
| :--------------------------------------: | :---------------------------------------------------: |
|   Templates/ Blueprints for containers   |            The running "unit of software"             |
| Contains code + required tools/ runtimes | Multiple containers can be created based on one image |

- Using Pre-Built & Custom Images
  - [Docker Hub](https://hub.docker.com/)
  - A container is base on an image
- Creating & Managing Containers

![image-layers](diagrams/image-layers.png)

&nbsp;

![image-containers](diagrams/image-containers.png)

|                         Images                         |                      Containers                      |
| :----------------------------------------------------: | :--------------------------------------------------: |
| Can be <b>tagged</b> (named) <i>-t, docker tag ...</i> |          Can be <b>named</b> <i>--name</i>           |
|       Can be <b>listed</b> <i>docker images</i>        | Can be <b>configured in detail</b> see <i>--help</i> |
|   Can be <b>analyzed</b> <i>docker image inspect</i>   |        Can be <b>listed</b> <i>docker ps</i>         |
| Can be <b>removed</b> <i>docker rmi, docker prune</i>  |        Can be <b>removed</b> <i>docker rm</i>        |

- Image tags (name : tag)
  - <b>name: </b>Defines a <b>group</b> of, <b>possible more specialized</b>, images (e.g. "node")
  - <b>tag: </b>Defines a <b>specialized image within a group of images</b> (e.g. "14")
- Specify the version (tag) to use
- Docker Hub
  - Official Docker Image Registry
  - Pubic, private and "official" mages
- Private Registry
  - Any provider/registry you want to use
  - Only your own (or team) images <b>(Needs to be HOST:NAME to talk to private registry)</b>
  - Share: `docker push IMAGE_NAME`
  - Use: `docker pull IMAGE_NAME`

&nbsp;

---

&nbsp;

> <b>ahmet: </b>Pulling and running image
> Let's say I have pulled an image from Dockerhub which is a webserver listening a port, how can I know that which port it's listening without seeing code? Maybe we'll cover that in future lessons..
>
> <b>Maximilian: </b>That's why you typically should document that via EXPOSE in your Dockerfile. If you pull some image where you never saw the Dockerfile, it should be documented on Docker Hub.

&nbsp;

---

&nbsp;

## Data & Volumes

|                  Application                  |                 Temporary App Data                  |                    Permanent App Data                     |
| :-------------------------------------------: | :-------------------------------------------------: | :-------------------------------------------------------: |
|       Application (Code + Environment)        |    Temporary App Data (e.g. entered user input)     |            Permanent App Data (user accounts)             |
|  Written & provided by you (= the developer)  |       Fetched / Produced in running container       |          Fetched / Produced in running container          |
|  Added to image and container in build phase  |         Stored in memory or temporary files         |               Stored in files or a database               |
| “Fixed”: Can’t be changed once image is built |     Dynamic and changing, but cleared regularly     |      Must not be lost if container stops / restarts       |
|       Read-only, hence stored in Images       | Read + write, temporary, hence stored in Containers | Read + write, permanent, stored with Containers & Volumes |

- Volumes are folders on your host machine hard drive which are mounted (“made available”, mapped) into containers
  - Anonymous
  - Named
- Host (Your Computer) /some-path <---> /app/user-data
- Volumes persist if a container shuts down. If a container (re-)starts and mounts a volume, any data inside of that volume is available in the container.
- A container can write data into a volume and read data from it.
- <b>Bind Mounts </b>are great for persistent and editable data
- For Windows using WSL Tool, there is a need to access Linux filesystems

|                      Command                      | Persist State |
| :-----------------------------------------------: | :-----------: |
|        `docker run -v /app/data ...</code>        | Anonymous V`  |
|     `docker run -v data:/app/data ...</code>      |  Named Vol`   |
| `docker run -v /path/to/code:/app/code ...</code> |   Bind Mou`   |

|                       Anonymous Volumes                        |                         Named Volumes                          |                           Bind Mounts                            |
| :------------------------------------------------------------: | :------------------------------------------------------------: | :--------------------------------------------------------------: |
|          Created specifically for a single container           |    Created in general – not tied to any specific container     | Location on host file system, not tied to any specific container |
|   Survives container shutdown / restart unless --rm is used    | Survives container shutdown / restart – removal via Docker CLI |    Survives container shutdown / restart – removal on host fs    |
|              Can not be shared across containers               |                Can be shared across containers                 |                 Can be shared across containers                  |
| Since it’s anonymous, it can’t be re-used (even on same image) |      Can be re-used for same container (across restarts)       |       Can be re-used for same container (across restarts)        |

- <b>Read-only Volume</b>
- We remove `COPY . .</code> in the dockerfile while using bind mount run command but we will not be using it when we are in the prod`

&nbsp;

---

&nbsp;

> <b>Lin: </b>Why do we not use bind mounts in production?

> <b>Adam: </b>You don't want to use bind mounts in production because they aren't very portable. If I gave you a command to start a container with an absolute path to the volume then you wouldn't be able to use it without editing it for your filesystem. Named volumes don't have that problem.

&nbsp;

---

&nbsp;

- Docker supports build-time <b>ARGuments</b> and runtime <b>ENVironment</b> variables

|                                      ARG                                      |                         ENV                          |
| :---------------------------------------------------------------------------: | :--------------------------------------------------: |
| Available inside of Dockerfile, NOT accessible in CMD or any application code | Available inside of Dockerfile & in application code |
|               Set on image build (docker build) via --build-arg               | Set via ENV in Dockerfile or via --env on docker run |

> <b>Environment variables and security: </b>Depending on which kind of data you're storing in your environment variables, you might not want to include the secure data directly in your Dockerfile.
>
> Instead, go for a separate environment variables file which is then only used at runtime (i.e. when you run your container with docker run). Otherwise, the values are "baked into the image" and everyone can read these values via `docker history IMAGE<`
>
> For some values, this might not matter but for credentials, private keys etc. you definitely want to avoid that! If you use a separate file, the values are not part of the image since you point at that file when you run docker run. But make sure you don't commit that separate file as part of your source control repository, if you're using source control.

&nbsp;

---

&nbsp;

## Networking

- Container to WWW communication
- Container to local host machine
- Container to container communication

![containers-and-network-requests](diagrams/containers-and-network-requests.png)

&nbsp;

![docker-ip-resolving](diagrams/docker-ip-resolving.png)

> Docker Networks actually support different kinds of <b>"Drivers"</b> which influence the behavior of the Network. The default driver is the <b>"bridge" driver</b> - it provides the behavior shown in this module (i.e. Containers can find each other by name if they are in the same Network).
>
> The driver can be set when a Network is created, simply by adding the `--driver</code> `
>
> `docker network create --driver bridge my-net`
>
> Of course, if you want to use the "bridge" driver, you can simply omit the entire option since "bridge" is the default anyways. Docker also supports these alternative drivers - though you will use the "bridge" driver in most cases:
>
> <b>host: </b>For standalone containers, isolation between container and host system is removed (i.e. they share localhost as a network)
>
> <b>overlay: </b>Multiple Docker daemons (i.e. Docker running on different machines) are able to connect with each other. Only works in "Swarm" mode which is a dated / almost > deprecated way of connecting multiple containers
>
> <b>macvlan: </b>You can set a custom MAC address to a container - this address can then be used for communication with that container
>
> <b>none: </b>All networking is disabled.
>
> <b>Third-party plugins: </b>You can install third-party plugins which then may add all kinds of behaviors and functionalities
>
> As mentioned, the "bridge" driver makes most sense in the vast majority of scenarios.

&nbsp;

---

&nbsp;

> <b>John: </b>Swarm, outdated or out branded?
>
> You cited Swarm as "dated / almost deprecated way of connecting multiple containers". Interesting as Bret Fisher, Docker Captain who does most of Docker's release cycle updates for the community has a very different take. He prefers Swarm but realizes Kubes has overwhelmed the community with marketing.
>
> Secondary, Swarm is less complicated though Kubes is trying to uncomplicate itself. Kubes also has higher system requirements. If you have massive complexity and scale extrodinaire then yes, Kubes is a winning choice. It has been said the first rule of architecture is "Everything in software architecture is a tradeoff". Kubes is no exception, and Swarm is also no exception. Swarm is the best choice in many situations.
>
> P.S. When I got my Docker Enterprise course certificate, official training, the instructor just two years ago said that eight out of ten deployments were on Swarm. Also, Mirantis was going to do away with Swarm and due to customer input have pulled Swarm back into long term support plans. Remember, K.I.S.S.? Why do Kubes if it's not needed?
>
> <b>Maximilian: </b>Yeah, swarm can be easier to get started with - still, I'm not convinced by Swarm's future.
>
> You will find different opinions out there for sure but whilst Kubernetes is clearly under very active development, the same can't really be said for Swarm. You can use it and it might "not go anywhere" but it also doesn't look like it's really being embraced by large chunks of the community.
>
> This [article](https://medium.com/@markuman/is-docker-swarm-mode-eol-7a3f316116a3) is also quite interesting.
>
> Feel free to use whatever you personally prefer - Docker Swarm might do the trick of course. But I definitely see Kubernetes being and becoming more important.

&nbsp;

---

&nbsp;

## Multi-Containers App

- [Docker Hub Mongo image - Authentication](https://hub.docker.com/_/mongo/)

### goals-app

> <b>Bernard: </b>Better solution for the goals app
> A better solution is to use the proxy feature of create-react-app (based on webpack dev server). Add the following line to your frontend package.json:
>
> `"proxy": "http://goals-backend:80"`
>
> Then, you can stop mapping the port 80 from the backend. Modify the react App.js to connect to `localhost:3000` instead of `localhost`.
>
> In this setup, only the frontend app (port 3000) is exposed and all backend calls are proxied inside the container network. This is solution is more secure and ressemble better a setup which could be used in production.

&nbsp;

---

&nbsp;

## Docker Compose

- What Docker Compose is NOT
  - does NOT replace Dockerfiles for custom Images
  - does NOT replace Images or Containers
  - is NOT suited for managing multiple containers on different hosts (machines)
- Services (Containers)
  - Published Ports
  - Environment Variables
  - Volumes
  - Networks
- [Compose file versions and upgrading](https://docs.docker.com/compose/compose-file/compose-versioning/)

&nbsp;

---

&nbsp;

## Utility Containers

![utility-containers](diagrams/utility-containers.png)

> <b>Scott: </b>Utility Containers and Linux
> I wanted to point out that on a Linux system, the Utility Container idea doesn't quite work as you describe it. In Linux, by default Docker runs as the "Root" user, so when we do a lot of the things that you are advocating for with Utility Containers the files that get written to the Bind Mount have ownership and permissions of the Linux Root user. (On MacOS and Windows10, since Docker is being used from within a VM, the user mappings all happen automatically due to NFS mounts.)
>
> So, for example on Linux, if I do the following (as you described in the course):

```dockerfile
FROM node:14-slim
WORKDIR /app
```

```sh
$ docker build -t node-util:perm .
$ docker run -it --rm -v $(pwd):/app node-util:perm npm init

...

$ ls -la

total 16
drwxr-xr-x  3 scott scott 4096 Oct 31 16:16 ./
drwxr-xr-x 12 scott scott 4096 Oct 31 16:14 ../
drwxr-xr-x  7 scott scott 4096 Oct 31 16:14 .git/
-rw-r--r--  1 root  root   202 Oct 31 16:16 package.json
```

> You'll see that the ownership and permissions for the package.json file are "root". But, regardless of the file that is being written to the Bind Mounted volume from commands emanating from within the docker container, e.g. "npm install", all come out with "Root" ownership.
>
> <b>Solution 1: Use predefined "node" user (if you're lucky)</b>
> There is a lot of discussion out there in the docker community (devops) about security around running Docker as a non-privileged user (which might be a good topic for you to cover as a video lecture - or maybe you have; I haven't completed the course yet). The Official Node.js Docker Container provides such a user that they call "node".
>
> https://github.com/nodejs/docker-node/blob/master/Dockerfile-slim.template

```dockerfile
FROM debian:name-slim
RUN groupadd --gid 1000 node \
         && useradd --uid 1000 --gid node --shell /bin/bash --create-home node
```

> Luckily enough for me on my local Linux system, my "scott" uid:gid is also 1000:1000 so, this happens to map nicely to the "node" user defined within the Official Node Docker Image. So, in my case of using the Official Node Docker Container, all I need to do is make sure I specify that I want the container to run as a non-Root user that they make available. To do that, I just add:

```dockerfile
FROM node:14-slim
USER node
WORKDIR /app
```

> If I rebuild my Utility Container in the normal way and re-run "npm init", the ownership of the package.json file is written as if "scott" wrote the file.

```sh
$ ls -la

total 12
drwxr-xr-x  2 scott scott 4096 Oct 31 16:23 ./
drwxr-xr-x 13 scott scott 4096 Oct 31 16:23 ../
-rw-r--r--  1 scott scott 204 Oct 31 16:23 package.json
```

> <b>Solution 2: Remove the predefined "node" user and add yourself as the user</b>
> However, if the Linux user that you are running as is not lucky to be mapped to 1000:1000, then you can modify the Utility Container Dockerfile to remove the predefined "node" user and add yourself as the user that the container will run as:

```dockerfile
FROM node:14-slim

RUN userdel -r node

ARG USER_ID

ARG GROUP_ID

RUN addgroup --gid $GROUP_ID user

RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user

USER user

WORKDIR /app
```

> And then build the Docker image using the following (which also gives you a nice use of ARG):

```sh
$ docker build -t node-util:cliuser --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) .

$ docker run -it --rm -v $(pwd):/app node-util:cliuser npm init
$ ls -la

total 12
drwxr-xr-x  2 scott scott 4096 Oct 31 16:54 ./
drwxr-xr-x 13 scott scott 4096 Oct 31 16:23 ../
-rw-r--r--  1 scott scott  202 Oct 31 16:54 package.json
```

> [Reference to Solution 2 above](https://vsupalov.com/docker-shared-permissions/)
>
> Keep in mind that this image will not be portable, but for the purpose of the Utility Containers like this, I don't think this is an issue at all for these "Utility Containers"

> <b>Jim: </b>I have encountered this on our AWS ECS deployments. We create an unprivileged user account as part of the Docker build process, just like you have documented here. Then the app runs under the user account and not as root.

> <b>raymi: </b>The php volumes in docker compose config in "Adding PHP Container" section should have been "cached" instead of "delegated" because most often changes come from the host side for dev and "read-only" is mostly on containers side. Please let me know.. what you reckon ? agree or disagree and reasons or I missed something ? just providing a feedback for improvement. love the course and keep it up Max

> <b>Maximilian: </b>Here's a good comparison of delegated vs cached: https://tkacz.pro/docker-volumes-cached-vs-delegated/
>
> I favor delegated here because we don't need container writes (primarily log files) to be reflected back onto our host machine immediately. That's not important here. We do definitely need to ensure that changes on the host machine are immediately reflected inside of the container though.

> <b>Volkoff: </b>Found nice TLDR tip on stackoverflow to make whole "delegated-cached" issue clear
>
> Use `cached`: when the host performs changes, the container is in read only mode.
>
> Use `delegated`: when docker container performs changes, host is in read only mode.
>
> Use `default`: When both container and host actively and continuously perform changes on data.

&nbsp;

---

&nbsp;

## Laravel & PHP

![laravel-php-target-setup](diagrams/laravel-php-target-setup.png)

- [Docker Hub nginx image](https://hub.docker.com/_/nginx/)
- [Laravel installation](https://laravel.com/docs/9.x/installation)

- Instead of stating the `ports` under php service in `docker-compose.yaml` file:

```dockerfile
services:
  ...
  php:
    ...
    ports:
      - '3000:9000'
```

- We can change the port from 3000 to 9000 in the `nginx.conf` file like so `fastcgi_pass php:9000;`
- Because we have container to container communication via network instead of localhost

&nbsp;

---

&nbsp;

## Deployment

|                                                 Development                                                  |                                            Production                                             |
| :----------------------------------------------------------------------------------------------------------: | :-----------------------------------------------------------------------------------------------: |
|                                       Isolated, standalone environment                                       |                                 Isolated, standalone environment                                  |
|                               Reproducible environment, easy to share and use                                |                          Reproducible environment, easy to share and use                          |
|                                 Bind Mounts shouldn’t be used in Production!                                 |                   Containerized apps might need a build step (e.g. React apps)                    |
| Multi-Container projects might need to be split (or should be split) across multiple hosts / remote machines |                 Trade-offs between control and responsibility might be worth it!                  |
|              Containers should encapsulate the runtime environment but not necessarily the code              | A container should really work standalone, you should NOT have source code on your remote machine |
|             Use “Bind Mounts” to provide your local host project files to the running container              |                          Use COPY to copy a code snapshot into the image                          |
|                         Allows for instant updates without restarting the container                          |        Ensures that every image runs without any extra, surrounding configuration or code         |

![basic-standalone-nodejs-app](diagrams/basic-standalone-nodejs-app.png)

- <b>Hosting Providers</b>
  - Amazon Web Services (AWS)
  - Microsoft Azure
  - Google Cloud

|                        <b>Option 1: </b>Deploy Source                        |          <b>Option 2: </b>Deploy Built Image          |
| :--------------------------------------------------------------------------: | :---------------------------------------------------: |
|                        Build image on remote machine                         | Build image before deployment (e.g. on local machine) |
| Push source code to remote machine, `run docker build` and then `docker run` |               Just execute `docker run`               |
|                            Unnecessary complexity                            |         Avoid unnecessary remote server work          |

&nbsp;

---

&nbsp;

## AWS EC2

- <b>Reference: </b>dep-basic-nodeapp
- Scaling & managing availability can be challenging
- Performance (also during traffic spikes) could be bad
- Taking care about backups and security can be challenging
- A service that allows you to spin up and manage your own remote machines
  1. Create and launch EC2 instance, VPC and security group
  2. Configure security group to expose all required ports to WWW
  3. Connect to instance (SSH), install Docker and run container

1. Launch an instance for EC2
2. <b>Application and OS Images (Amazon Machine Image): </b>Amazon Linux AMI 64-bit (x86)
3. <b>Instance type: </b>t2.micro (Free tier eligible)
4. <b>Key pair: </b>Create a new key pair
   1. <b>Key pair name: </b>Enter key pair name
   2. <b>Key pair type: </b>RSA
   3. <b>Private key file format: </b>.pem
   4. Save the .pem file into the project root directory
   5. <b>Take note that anyone with the file will be able to connect to your remote machine</b>
5. <b>Network: </b>vpc
6. Launch instance
7. View all instances
8. <b>Select your instance, connect and follow the steps under SSH client</b>
9. `sudo yum update -y` to ensure all essential packages on the remote machine are updated
10. `sudo amazon-linux-extras install docker`
    - [Docker engine installation for other providers](https://docs.docker.com/engine/install/)
11. `sudo service docker start`
12. `docker tag CONTAINER darrela/CONTAINER` e.g. `docker tag dep-basic-nodeapp darrela/dep-basic-nodeapp`
13. Push image to Docker Hub
14. `sudo docker run --rm -dp 80:80 darrela/dep-basic-nodeapp`
    - Refer to Ryan's comment below for Apple M1
15. Under <b>Network & Security - Security Groups: </b>
    - Edit inbound rules
    - <b>Add rule: </b>HTTP & Anywhere-IPv4
    - Save rules
16. Go to Public IPv4 address
17. Use `sudo docker pull` to update the image after rebuilding and pushing to Docker Hub

- "DIY" Approach disadvantage
  - We fully “own” the remote machine è We’re responsible for it (and it’s security)!
    - Keep essentials software updated
    - Manage network and security groups/ firewall
  - SSHing into the machine to manage it can be annoying

&nbsp;

---

&nbsp;

> <b>Ryan: </b>The requested image's platform (linux/arm64/v8) does not match the detected host platform (linux/amd64) and no specific platform was requested
>
> If you built the Docker image on a MacBook with the M1 chip, and you try to run the Docker image on your EC2, you'll get the error above.
>
> To solve this, rebuild the image locally using this command:
>
> `docker buildx build --platform linux/amd64 -t node-dep-example .`
>
> You're essentially forcing the Docker image to be rebuilt using the specified architecture (linux/amd64) vs. using the detected architecture of your MacBook (linux/arm64/v8), which simply can't run on the selected EC2.
>
> Now tag the image with your Docker hub repository name as before:
>
> `docker tag node-dep-example <your-account>/node-example-1`
>
> And push the new image to Docker hub as before:
>
> `docker push <your-account>/node-example-1`
>
> Switch back to the EC2 and delete the local version of the Docker image it previously downloaded:
>
> `sudo docker rmi <your-account>/node-example-1`
>
> Last, run the newly built Docker image on the EC2, which should now work:
>
> `sudo docker run -d --rm -p 80:80 <your-account>/node-example-1`

&nbsp;

---

&nbsp;

## AWS ECS

|                                          EC2                                           |                                              ECS                                              |
| :------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------: |
| You need to create them, manage them, keep them updated, monitor them, scale them etc. | Creation, management, updating is handled automatically, monitoring and scaling is simplified |
|                  Great if you’re an experienced admin / cloud expert                   |                   Great if you simply want to deploy your app / containers                    |

- <b>Reference: </b>dep-basic-nodeapp

1. Get Started
2. <b>custom</b> Configure
3. <b>Container name: </b>dep-basic-nodeapp
4. <b>Image: </b>darrela/dep-basic-nodeapp
5. <b>Port mappings: </b>Refer to server.js/app.js port e.g. 80
6. <b>Log configuration: </b>Check Auto-configure CloudWatch Logs
7. Next (Optional: Application Load Balancer)
8. Next > Create > View Service
9. Task > Task ID > Public IP
10. <b>To update: </b>Push the new image to Docker Hub
11. Create new revision of Task Definition or directly under Actions click Update Service > Force new Deployment

&nbsp;

- <b>Reference: </b>dep-multi-containers
- Docker compose is good for local machine. But there will be limitations for deployment into the cloud whereby host providers may need different requirements and potentially there are multiple different machines working together.
- AWS ECS look for images from Docker Hub

1. ECS > Clusters > Create Cluster
2. Networking only > Next step
3. <b>Cluster name</b>
4. <b>Create VPC: </b>Create a new VPC for this cluster
5. Create > View Cluster > Task Definitions > Create new Task Definition > Next step
6. Input Task Definition Name
7. <b>Task Role: </b>ecsTaskExecutionRole
8. Input <b>Task memory (GB)</b> & <b>Task CPU (vCPU)</b>
9. <b>Container definitions: </b> Add container
10. <b>Container name</b> & <b>Image</b>
11. <b>Port mappings</b>
12. <b>ENVIRONMENT: </b>
    - <b>Command: </b>"node,app.js"
    - <b>Environment variables: </b> <i>MONGODB_URL</i> can be set as "localhost" on AWS ECS
      - The containers can communicate with each other under the "localhost" key
13. Add
14. Add remaining container(s)
15. Create > View task definition > Cluster > Services tab > Create
16. <b>Launch type: </b>FARGATE
17. Input Task Definition, Cluster, Service name & Number of tasks (1)
18. Input Cluster VPC, Subnets & Auto-assign public IP
19. <b>Load balancer type: </b>Application Load Balancer
    - If required, Create Application Load Balancer
      - input Load balancer name
      - Under target group, target type select IP addresses
      - Under Health check settings, <b>Path</b>: "/goals" <i>For dep-multi-containers</i>
      - <b>Security groups:</b> Default + Goals
20. <b>DNS Name: </b>ecs-lb-912341198.ap-southeast-1.elb.amazonaws.com
21. Using EFS Volumes with ECS
    - Task Definitions > Create new revision > Volumes (Add Volume)
    - <b>Name: </b>Data
    - <b>Volume type: </b>Elastic File System (EFS)
    - Go to Amazon EFS console > Create file system
    - Select VPC > Customize > Next
    - Go to EC2 on a new tab > Security Groups > Create security group
    - <b>Add Inbound rules: </b>NFS & pick your Custom <i>Goals</i> Source
    - Without the security group and inbound rule, the containers and tasks in ECS would not be able to communicate with EFS
    - Create security group
    - Back at file system, under Network access settings pick the new security group
    - Next > Next > Create
    - Back at Amazon ECS (Add volume modal) under <b>File system ID</b> select the new file system > Add
    - Click database <i>(mongodb)</i> > STORAGE AND LOGGING > Mount points
      - <b>Source volume: </b>`data`
      - <b>Container path: </b>`/data/db` (Will need to change if using mysql or other dialects)
    - Update > Create > Actions > Update Service > Force new deployment > Skip to review > Update Service > View Service

![dep-current-app-architecture](diagrams/dep-current-app-architecture.png)

1. Use MongoDB Atlas
   - If required remove from ECS
     - mongodb container
     - volume from
     - At Amazon EFS, delete file system
     - At EC2, delete Security group
2. Update backend container environment variables
3. Create > Actions > Update Service > Force new deployment > Skip to review > Update Service > View Service

![dep-mongodb-atlas-architecture](diagrams/dep-mongodb-atlas-architecture.png)

&nbsp;

---

&nbsp;

> <b>Ivo: </b> Some other considerations when running this setup in an actual production environment
>
> I just wanted to add some other considerations when running this setup (development & production database on an external machine/cluster):
>
> - running a development database on some external server can get very cumbersome when you start writing tests for your application. It can add considerable latency to test related tasks like seeding the database with test data and in general running tests that require a database connection
> - using the same database machine/cluster for development as well as production with the only difference being the database name will inevitably lead to someone making a mistake with the database name and overwriting the entire production database with development data
>
> I do realize that for the sake of simplicity and conformity in database versions you chose to take this path Max. I also realise that my considerations are somewhat outside the scope of this course, but on the other hand: I guess if you are taking this course as a student, you are likely planning to someday put the learned information to use in an actual production environment ;)

&nbsp;

---

&nbsp;

- Apps with Development Servers & Build Steps
  - Some apps / projects require a build step e.g. optimization script that needs to be executed AFTER development but BEFORE deployment

![apps-with-development-servers-and-build-steps](diagrams/apps-with-development-servers-and-build-steps.png)

- Multi-Stage Builds
  - One Dockerfile, Multiple Build / Setup Steps (“Stages”)
  - Stages can copy results (created files and folders) from each other
  - You can either build the complete image or select individual stages
  - You can use double `FROM` in the Dockerfile
    - Refer to `Dockerfile.prod` file in frontend folder in dep-multi-containers folder

1. Task Definitions > Create new revision > Add container
2. Input Container name, Image & Port mappings
3. STARTUP DEPENDENCY ORDERING set backend container name with Condition "SUCCESS"
4. Add
5. We will need a new Task Definition if both frontend and backend are listening to the same port.
   - Input Task definition name, Task role, Task memory (GB), Task CPU (vCPU)
   - This means we will have 2 different URLs for both frontend and backend.
6. Setup a new load balancer for frontend at EC2 page > Create Application Load Balancer
   - Input Load balancer name, VPC under network mapping & Security groups
   - Setup a new target group with target type IP addresses
7. Create load balancer > Copy DNS name at EC2 Load Balancer page
8. Input `backendUrl` in `App.js` file in frontend folder in dep-multi-containers folder
9. After rebuilding frontend image, push to Docker Hub
10. At ECS page, from Task Definitions, Create Service for frontend
11. <b>Launch type: </b>FARGATE
12. Input Service name, Number of tasks then Next Step
13. Input Cluster VPC, Subnets & Security groups
14. <b>Load balancer typer: </b>Application Load Balancer
15. Input Load balancer name > Add to load balancer
16. Select Target group name > Next Step > Next Step > Create Service

&nbsp;

---

&nbsp;

> <b>Javed: </b>Why do we need an nginx server for react code?
>
> I thought react just builds a bundle.js file thats served to the user from the express server. Why do we need an express server and an nginx server?

> <b>Maximilian: </b>You can use Express.js to serve your React app. But if you just build a React app for production, it doesn't come without any default server. It only has a development server (based on NodeJS) during development - you can't use that (or you shouldn't) for production.
>
> Hence you need to bring your own server for the production build. Either your own Express server, sure, or - if you don't want to write all that code - simply a Nginx server.

&nbsp;

---

&nbsp;

- Develop your application in the same environment you’ll run it in after deployment

|                   Local Host / Development                    |                       Remote Host / Production                       |
| :-----------------------------------------------------------: | :------------------------------------------------------------------: |
| Isolated, encapsulated, reproducible development environments |          Isolated, encapsulated, reproducible environments           |
|               No dependency or software clashes               | Easy updates: Simply replace a running container with an updated one |

- It’s perfectly fine to use Docker (and Docker Compose) for local development!
  - Encapsulated environments for different projects
  - No global installation of tools
  - Easy to share and re-produce
- Deployment Considerations
  - Replace Bind Mounts with Volumes or COPY
  - Multiple containers might need multiple hosts
  - But they can also run on the same host (depends on application)
  - Multi-stage builds help with apps that need a build step
  - Control vs Ease-of-use
    - <b>Remote server, install Docker and run your containers: </b>Full control but you also need to manage everything
    - <b>Managed service: </b>Less control and extra knowledge required but easier to use, less responsibility

&nbsp;

---

&nbsp;

## Kubernetes Basics

- [Kubernetes](https://kubernetes.io/), also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.
- Manual deployment of Containers is hard to maintain, error-prone and annoying
- Even beyond security and configuration concerns

|                          Problem                           |                 AWS ECS Solution                  |
| :--------------------------------------------------------: | :-----------------------------------------------: |
|  Containers might crash / go down and need to be replaced  | Container health checks + automatic re-deployment |
| We might need more container instances upon traffic spikes |                    Autoscaling                    |
|       Incoming traffic should be distributed equally       |                   Load balancer                   |

- Using a specific cloud service locks us into that service
- You need to learn about the specifics, services and config options of another provider if you want to switch
- Just knowing Docker isn’t enough!

&nbsp;

- <b>Kubernetes: </b>An open-source system (and de-facto standard) for orchestrating container deployments
  - Automatic Deployment
  - Scaling & Load Balancing
  - Management
- <b>Why?</b>
  - Kubernetes Configuration (i.e. desired architecture – number of running containers etc.)
    - Standardized way of describing the to-be-created and to-be-managed resources of the Kubernetes Cluster
    - Cloud-provider-specific settings can be added
  - Some Providerspecific Setup or Tool
  - Any Cloud Provider or Remote Machines (e.g. could also be your own datacenter)
- Kubernetes is like Docker-Compose for multiple machines

|                            IS NOT                            |                   IS                    |
| :----------------------------------------------------------: | :-------------------------------------: |
|              It’s not a cloud service provider               |       It’s an open-source project       |
|        It’s not a service by a cloud service provider        |    It can be used with any provider     |
| It’s not restricted to any specific (cloud) service provider |    It can be used with any provider     |
|       It’s not just a software you run on some machine       | It’s a collection of concepts and tools |
|              It’s not an alternative to Docker               |    It works with (Docker) containers    |
|                   It’s not a paid service                    |     It’s a free open-source project     |

![kubernetes-architecture](diagrams/kubernetes-architecture.png)

|                                What Kubernetes Will Do                                 |                  What You Need To Do / Setup (i.e. what Kubernetes requires)                   |
| :------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------: |
|                    Create your objects (e.g. Pods) and manage them                     |               Create the Cluster and the Node Instances (Worker + Master Nodes)                |
|                    Monitor Pods and re-create them, scale Pods etc.                    |          Setup API Server, kubelet and other Kubernetes services / software on Nodes           |
| Kubernetes utilizes the provided (cloud) resources to apply your configuration / goals | Create other (cloud) provider resources that might be needed (e.g. Load Balancer, Filesystems) |

![worker-node.png](diagrams/worker-node.png)

&nbsp;

![master-node.png](diagrams/master-node.png)

|             |                                                         Core Components                                                         |
| :---------: | :-----------------------------------------------------------------------------------------------------------------------------: |
|   Cluster   |   A set of Node machines which are running the Containerized Application (Worker Nodes) or control other Nodes (Master Node)    |
|    Nodes    | Physical or virtual machine with a certain hardware capacity which hosts one or multiple Pods and communicates with the Cluster |
| Master Node |                                  Cluster Control Plane, managing the Pods across Worker Nodes                                   |
| Worker Node |                                        Hosts Pods, running App Containers (+ resources)                                         |
|    Pods     |                     Pods hold the actual running App Containers + their required resources (e.g. volumes).                      |
| Containers  |                                                   Normal (Docker) Containers                                                    |
|  Services   |                      A logical set (group) of Pods with a unique, Pod- and Containerindependent IP address                      |

&nbsp;

---

&nbsp;

> <b>The: </b>Hi! I have a question about pods
>
> If pod can hold multiple containers, so why we need to use multiple pods in the worker node? Why don't we put all the containers that we use in a "service" in a single pod?

> <b>Zhing Jieh Jack: </b>One important characteristic I can think of off the top of my head is: A pod resides in a worker node; If we put all types of containers e.g frontend, backend, database, into a pod, then there's a single point of failure. Placing those containers in separate pods allow them to be in diff machine/worker node, eg frontend, backend and database each may reside in diff machines/nodes.
>
> And what if I (administrator) want to scale only the frontend? We certainly don't want to scale other "services/containers" in this case.
>
> I believe that a K8s pod is meant to do one thing instead of multiple different things. Of course that one thing can be multiple containers (a sidecar proxy). But for a collection of containers doing different things (e.g frontend, backend, database) they should each be in a separate pod.
>
> I'm sure there are other reasons, i'm happy to have anyone add on to my answer :)

&nbsp;

---

&nbsp;

- Kubernetes works with objects
  - Created <b>imperatively</b> or <b>declaratively</b>

### Pod Object

- The smallest “unit” Kubernetes interacts with
  - Contains and runs one or multiple containers
    - The most common usecase is “one container per Pod”
  - Pods contain shared resources (e.g. volumes) for all Pod containers
  - Has a cluster-internal IP by default
    - Containers inside a Pod can communicate via localhost
- Pods are designed to be ephemeral: Kubernetes will start, stop and replace them as needed.
- For Pods to be managed for you, you need a “Controller” (e.g. a “Deployment”)
  - The volume inside the pod gets erased

### Deployment Object

- Controls (multiple) Pods
  - You set a desired state, Kubernetes then changes the actual state
    - Define which Pods and containers to run and the number of instances
  - Deployments can be paused, deleted and rolled back
  - Deployments can be scaled dynamically (and automatically)
    - You can change the number of desired Pods as needed
- Deployments manage a Pod for you, you can also create multiple Deployments
- You therefore typically don’t directly control Pods, instead you use Deployments to set up the desired end state

![kubectl-behind-the-scene](/diagrams/kubectl-behind-the-scene.png)

### Service Object

- Exposes Pods to the Cluster or Externally
  - Pods have an internal IP by default – it changes when a Pod is replaced
    - Finding Pods is hard if the IP changes all the time
  - Services group Pods with a shared IP
  - Services can allow external access to Pods
    - The default (internal only) can be overwritten
- Without Services, Pods are very hard to reach and communication is difficult
- Reaching a Pod from outside the Cluster is not possible at all without Services

&nbsp;

---

&nbsp;

> <b>.</b>I'm coding along on video#193. I rolled back the revision, but the browser is still showing the wrong version in the UI.
>
> As per the video#193, I deployed the second version of the image, then I tried to roll back to first version again using `kubectl rollout undo deployment/first-app --to-revision=1`. The rollout is successful & I can see the same in dashboard too, but the app page is still showing the revision 2. Am I doing anything wrong?

> <b>Justin</b>If you were working along these in tandem with Max, you likely did the same thing I did and pushed the updated version of app.js to `:latest`(see ~3:05 from the previous video) before he began explaining the need to provide a unique tag.
>
> It seems like an important thing to note that undoing a rollout in kubectl does not return a cached version of that revision, but instead re-fetches the image from registry with whatever tag that revision had associated to it.
>
> Doing some googling, it looks like an image's Digest is the only way to reflect an immutable image snapshot [(docs)](https://docs.docker.com/engine/reference/commandline/pull/#pull-an-image-by-digest-immutable-identifier). I tested passing the sha256 from 2 subsequent `docker push` calls for the image during update, and kubectl took the revision successfully (and respectively rolled back as expected). As someone newly exploring docker/k8s, I really hope production services are relying on these digests for deployments rather than the tags themselves (at least when using external dependencies that they cannot easily revert). There doesn't seem to be a lot of chatter around this being the pragmatic approach, however.
>
> EDIT: It looks like referencing the cached version is based on the `imagePullPolicy`. Since the first version did not have an explicit tag (or using `:latest`), Kubernetes applies an `Always` policy. In other words, if you're using explicit tags, you should be pulling from the cached version of that image if found locally unless you specify otherwise [(docs)](https://kubernetes.io/docs/concepts/configuration/overview/#container-images). I'm not sure if this helps in a production setting, as I'd imagine the deployment VM is instantiated in a completely different host when auto scaling / load balancing.

&nbsp;

---

&nbsp;

|                               Imperative                               |                           Declarative                            |
| :--------------------------------------------------------------------: | :--------------------------------------------------------------: |
|                     `kubectl create deployment …`                      |                  `kubectl apply –f config.yaml`                  |
| Individual commands are executed to trigger certain Kubernetes actions | A config file is defined and applied to change the desired state |
|                 Comparable to using `docker run` only                  |     Comparable to using `Docker Compose` with compose files      |

- Set resource definition in .yaml file
- [Kubernetes - Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Kubernetes API - Deployment](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/deployment-v1/)
- [Kubernetes API - Service](https://kubernetes.io/docs/reference/kubernetes-api/service-resources/service-v1/)

> <b>Javed: </b>`ImagePullPolicy: Always` doesn't work for me unless deployment is deleted
>
> After checking on stackoverflow, I read setting it to always won't force a pull unless you shutdown and the start the deployment again. I tried following your instructions and it just said:

```sh
# kub-action-01-starting-setup$ kubectl apply -f=deployment.yml,service.yml
# deployment.apps/second-app-deployment unchanged
# service/backend unchanged
```

> So my new code wasnt reflected until I did a kubectl delete followed by apply

> <b>Maximilian: </b>That is true - sorry for the confusion caused and thanks for sharing this, much appreciated!

> <b>Émerson: </b>`kubectl rollout restart deployment/demo`
>
> [Source](https://stackoverflow.com/questions/40366192/kubernetes-how-to-make-deployment-to-update-image)

&nbsp;

---

&nbsp;

## Kubernetes Data & Volumes

- [Kubernetes - Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)
- Mount Volumes into Containers
  - A broad variety of Volume types / drivers are supported
    - ”Local” Volumes (i.e. on Nodes)
    - Cloud-provider specific Volumes
  - Volume lifetime depends on the <b>Pod lifetime</b>
    - Volumes survive Container restarts (and removal)
    - Volumes are removed when Pods are destroyed

|                   Kubernetes                    |                     Docker                      |
| :---------------------------------------------: | :---------------------------------------------: |
|    Supports many different Drivers and Types    |       Basically no Driver / Type Support        |
|     Volumes are not necessarily persistent      |     Volumes persist until manually cleared      |
| Volumes survive Container restarts and removals | Volumes survive Container restarts and removals |

- <b>Reference: </b>story-app
- We will get the error `"message": "Failed to open file."`, when we `GET` the route `/error` while using more than 1 replica for deployment of `story-app`.
  - Because the traffic has been redirected to another pod since we have error in the first pod.
  - We can switch the `emptyDir` volume type in `deployment.yaml` file to [`hostPath`](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath).
  - This allows multiple Pods to share the same path under the same host machine.
  - Unlike `emptyDir`, it does not create an empty path so we need to specify the `path`.
  - `hostPath` partially works around that in “OneNode” environments
- [Kubernetes - Volume CSI](https://kubernetes.io/docs/concepts/storage/volumes/#csi)

  - [Container Storage Interface (CSI) for Kubernetes GA](https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/)
  - Flexible volume type which allows you to attach any storage solution, provided that there is intergration support for this type.
  - e.g. [Amazon EFS CSI Driver](https://github.com/kubernetes-sigs/aws-efs-csi-driver)

![persistent-volumes-pv-claims](diagrams/persistent-volumes-pv-claims.png)

- <b>Persistent Volume</b> allows independency from the node
- [PV resource model](https://github.com/kubernetes/design-proposals-archive/blob/main/scheduling/resources.md)
- [Storage pros and cons: Block vs file vs object storage](https://www.computerweekly.com/feature/Storage-pros-and-cons-Block-vs-file-vs-object-storage)

|                  ”Normal” Volumes                   |                       Persistent Volumes                        |
| :-------------------------------------------------: | :-------------------------------------------------------------: |
|     Volume is attached to Pod and Pod lifecycle     | Volume is a standalone Cluster resource (NOT attached to a Pod) |
|        Defined and created together with Pod        |              Created standalone, claimed via a PVC              |
| Repetitive and hard to administer on a global level |           Can be defined once and used multiple times           |

&nbsp;

---

&nbsp;

> <b>Armin: </b>Wouldn't there be a concurrency problem with multiple pods/containers accessing the same file?
>
> If I start, let's say, 3 pods, and I get requests on all 3 pods simultaneously, wouldn't that possibly cause problems with writing to the same file on different containers simultaneously? Would it not possibly cause an error when one pod writes to a file, where it is usually locked then, and the other container also tries to write to the same file simultaneously?

> <b>Joel: </b>Yes this is not something you would do in production, instead you would use a single pod (like a database) with a persistent volume to share data between replicas.

&nbsp;

---

&nbsp;

## Kubernetes Networking

- <b>Reference: </b>task-app
- [CoreDNS](https://coredns.io/)
- In most cases, you do not want multiple containers per pod even though you can do it
- And you should only do that if the containers are tightly coupled with each other
- <b>Reverse Proxy for the Frontend: </b>refer to `nginx.conf` file line 4 to 6

![task-app](/diagrams/task-app-pod-internal.png)
![task-app](/diagrams/task-app-cluster-internal.png)

&nbsp;

---

&nbsp;

## Kubernetes Deployment

- <b>Reference: </b>dep-aws-eks
- Custom Data Center
  - Install + configure everything on your own
    - Machines
    - Kubernetes Software
- Cloud Provider
  - Install + configure most things on your own
    - Create + connect machines
    - Install + configure software
    - Manually or via [kops](https://github.com/kubernetes/kops) etc
  - Use a managed service
    - Define cluster architecture
    - Services like AWS EKS

|         AWS EKS (Elastic Kubernetes Service)         |    AWS ECS (Elastic Container Service)     |
| :--------------------------------------------------: | :----------------------------------------: |
|      Managed service for Kubernetes deployments      | Managed service for Container deployments  |
|    No AWS-specific syntax or philosophy required     | AWS-specific syntax and philosophy applies |
| Use standard Kubernetes configurations and resources | Use AWS-specific configuration andconcepts |

### AWS EKS

1. Create Cluster + Nodes
2. Connect kubectl to AWS EKS Cluster
3. `kubectl apply …`
4. Optional: Add additional AWS resources (e.g. AWS EFS)

&nbsp;

---

&nbsp;

> <b>Brenton: </b> How do you remove AWS resources?
>
> In case this helps someone else, AWS Support got back to me with this [link](https://docs.aws.amazon.com/eks/latest/userguide/delete-cluster.html) for instructions on how to remove resources for k8s clusters:
>
> As before, following the instructions and trying to delete them through the AWS Console did not work and I kept running into errors that didn't make sense and what looked like hanging operations when trying to delete resources.
>
> Finally had some luck after installing the eksctl cli tool on my machine and running the following commands:
> (first delete services with an external IP address assigned with kubectl):

```sh
kubectl get svc --all-namespaces (check which ones have external IP values)
kubectl delete svc users-service
eksctl delete cluster --name kub-dep-demo
```

> I'm asking for confirmation from AWS if that really for real deleted everything and I won't get zombie instances (and charges) spinning back up. This was an expensive lesson, so hopefully it will save someone else from the trouble and surprise charges.
>
> <b>Update: </b>confirmed that worked. If you have trouble in the gui console, just use eksctl.

&nbsp;

---

&nbsp;

1. Add cluster > Create > Cluster configuration
2. Create Cluster service role if required
   - [IAM is Free](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
   - IAM Console > Roles > Create role > AWS Service > <b>EKS - Cluster</b> > Next > Next
   - <b>Role name: </b>eksClusterRole
   - Create role
3. <b>Cluster service role: </b>eksClusterRole
4. Next > Open CloudFormation in new tab > Create stack
   - From [Creating a VPC link](https://docs.aws.amazon.com/eks/latest/userguide/creating-a-vpc.html#create-vpc),
   - copy & paste `IPv4` url into <b>Amazon S3 URL</b> under <b>Specify template</b>
   - Next > Input <b>Stack name</b> > Next > Create stack
5. Back at creating EKS cluster select the new VPC
6. <b>Cluster endpoint access</b> pick "Public and private"
7. Next > Next > Create
8. Back on your pc, navigate to your `.kube` hidden folder from your user folder and open the `config` file
   - Duplicate the `config` and name it as `config.minikube` so we can talk to minikube again.
   - Edit current `config` file to communicate with EKS with [AWS CLI](https://aws.amazon.com/cli/)
   - Open profile `Security credentials` in a new tab
   - Under `Access keys (access key ID and secret access key)` > Create New Access Key > Download Key File
   - In your shell, input `aws configure`
   - Input <b>AWS Access Key ID</b>, <b>AWS Secret Access Key</b> & <b>Default region name</b>: "ap-southeast-1"
   - Enter > Check that <b>Cluster info status</b> is "Active"
   - In your shell input `aws eks --region ap-southeast-1 update-kubeconfig --name dep-aws-eks`
     - The config is now set for communicating with <b>AWS EKS Cluster</b> instead of <b>Minikube Cluster</b>
9. Back Cluster Detail, <b>Compute tab</b>, click <b>Add node group</b>
10. <b>Configure node group: </b>Input Name & Node IAM Role
11. Create Node IAM role if required
    - The worker node which is essentially part of EC2 instances require permission such as logging or connect to some services
    - IAM Console > Roles > Create role > AWS Service > <b>EC2</b> > Next
    - Add <b>"AmazonEKSWorkerNodePolicy"</b>, "<b>AmazonEKS_CNI_Policy"</b> & <b>"AmazonEC2ContainerRegistryReadOnly"</b>
    - <b>Role name: </b>eksNodeGroup
    - Create role
12. <b>Node IAM role: </b>eksNodeGroup
13. Next > Select an instance
    - t3.micro is the cheapest
    - But select t3.small as schedulling pods on t3.micro might fail and application can be stuck in the pending state
14. Next > Next > Create
    - This will spin up a couple of EC2 instances and add them to the cluster
    - EKS will take care of launching, installing packages required by Kubernetes like kubelet and kube proxy on these nodes
    - And add all these into the cluster network
15. Check that <b>Node group configuration</b> is "Active"
16. There will be no Load Balancer at this point of time
17. Load Balancer will be created automatically by AWS at the later stage
18. Cluster has been setup just like Minikube except that it is not on a VM on our local machine

![EC2-resources-setting-up-EKS](/diagrams/EC2-resources-setting-up-EKS.png)

### Adding EFS as a Volume (with the CSI Volume Type)

- [kubernetes-sigs/aws-efs-csi-driver](https://github.com/kubernetes-sigs/aws-efs-csi-driver)
  - Run the deploy the stable driver command under "Installation" section
  - We need this EFS driver since AWS EFS is not supported as a volume type otherwise

1. EC2 > Security Groups > Create security group
2. <b>Input Security group name: <b> e.g. "eks-efs"
3. <b>VPC: <b>Select respective vpc e.g. "eksVpc"
4. <b>Inbound rules: </b>Type: NFS & Source: Custom
   - Input the "IPv4 CIDR" from the VPC page
5. Create security group
6. At Aamazon EFS > Create file system
7. Input name & select your eksVpc > Customize
8. Under the Security groups (eks-efs security group) > Next > Next > Create
9. Copy "File system ID" (fs-00bf054bf589c6c31)
10. Add your .yaml file `kind: PersistentVolume` details
    - [examples/kubernetes/static_provisioning](https://github.com/kubernetes-sigs/aws-efs-csi-driver/tree/master/examples/kubernetes/static_provisioning)

&nbsp;

---

&nbsp;
