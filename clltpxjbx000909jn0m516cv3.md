---
title: "A Step-by-Step guide to Build and Push Your Own Docker Images to DockerHub"
seoTitle: "Build and Push Your Own Docker Images to Docker Hub"
datePublished: Sun Aug 27 2023 17:22:29 GMT+0000 (Coordinated Universal Time)
cuid: clltpxjbx000909jn0m516cv3
slug: a-step-by-step-guide-to-build-and-push-your-own-docker-images-to-dockerhub
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693175681791/06331119-7edd-4af6-88ed-3f610cf97f33.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1693158032807/47f3476b-8162-4ec9-b038-ca2523c02780.webp
tags: docker, nginx, docker-images, dockerhub, docker-build

---

## Overview

Docker has revolutionized the way we deploy applications by making it easier to package and distribute software along with all its dependencies. While Docker offers a vast repository of pre-built images, there's immense value in creating your own custom Docker images tailored to your specific needs. In this guide, we'll walk through the process of creating your own Docker image, starting from scratch, and we'll even explore how to publish it on Docker Hub for easy sharing. We are going to create a docker image which is a tiny web server to serve out a web page through nginx.

## But what is Docker?

Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime, so you don't need to rely on what's installed on the host. Using Docker, you can quickly deploy and scale applications into any environment and know your code will run and thus, it helps to avoid "It works on my machine" problem.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693102921810/891343ad-462c-4db6-97d3-ce644af8ec54.png align="center")

## How Docker works - A simple explanation

Docker is an operating system for containers. It works by providing a standard way to run your code. Similar to how a virtual machine virtualizes server hardware, containers virtualize the operating system of a server. Docker is installed on each server and provides simple commands you can use to build, start, or stop containers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693108774508/8028a909-78b8-48b9-b9e6-9b97af84de8d.png align="center")

## Implementation

### Create a Dockerfile

Our first step is to create a Dockerfile. A Dockerfile is a script that contains instructions for building a Docker image. It's essentially a blueprint for your image.

Create a new directory, example- *docker-project* and open that directory in Visual Studio Code. Create a new file called *Dockerfile.* You can make use of my repo: [https://github.com/komal-max/Build-and-Push-Your-Own-Docker-Images](https://github.com/komal-max/Build-and-Push-Your-Own-Docker-Images)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693109929600/47125d4b-dc7a-4dee-886d-3fa2463ec0c7.png align="center")

Almost every single image for docker is based on another image and most of them are linux. You can specify an image like 'ubuntu' and this will give you a good starting point with all the dependencies from ubuntu to build your image, but if we are building a web server which is the case for this project, we would want to base it off something smaller like nginx. When we set our base image from nginx, it is actually coming from dockerhub: [https://hub.docker.com/\_/nginx](https://hub.docker.com/_/nginx), where we can see many different tags for this image like version tags, unversioned tags, and some that include additional dependencies like perl or we have something that is based off an entirely different base image altogether like alpine. We can add any tag visible on dockerhub for this image, for this I went for 'latest' tag, if you would like to have a smaller image size, it is much better to go for 'alpine'. To see the difference, use command docker images in terminal and you can see a significant difference in size of the image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693114134491/81cc5157-db00-4ccc-8708-fe62572831ae.png align="center")

Next, we are going to use *COPY* command, the *COPY* command can copy files from host machine into our actual image, means for us, copying in our website or some static pages. For this, we will first create a *source* folder and in that *source* folder, we will create a new folder called *html* and in there let's create an *index.html.* Please feel free to use the simple html file in my github repository. I have kept it to minimal for the sake of an example, feel free to customize it. We can also add a couple of images in our webpage.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693114992412/adfa35de-ae96-47ac-8249-10a7069f9ac5.png align="center")

Now, going back to our Dockerfile, *COPY* command takes two parameters, source and destination.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693115578391/1f4ff521-dad5-477a-885c-2271da8bb3f9.png align="center")

Next, we can expose port 80, but we can leave this step as when we use this image, we can expose any port we want.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693116205681/50b467bd-c169-4896-834d-54d3bf66dbf8.png align="center")

Next, we need to define a command for entrypoint, we also want to pass some arguments in the form of an array of parameters

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693116419361/1ac048a7-8863-45a7-826a-1495ebf0994e.png align="center")

### How to build an image?

Open terminal and check if you have docker installed using commands like *docker --version* or *docker info.* If not, make sure to install docker from: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693109072331/22d3e6a6-1762-4b41-8687-0c8e1d728967.png align="center")

Now, cd to the folder that has the *Dockerfile.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693116992797/a0edecc8-084f-427e-b29c-783c1697d840.png align="center")

To build the image with a tag, use command: *docker build -t &lt;name of your image&gt; &lt;a path to the docker file which is this current directory, hence we use (****.****)&gt;*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693117143046/9fec24dd-49fc-4f8f-ad56-bd79448e3e6c.png align="center")

To check our image, use *docker images*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693117358108/68f46bb4-a087-44a8-b4fc-e292b610a200.png align="center")

***How do we create a container now?***

An image becomes a container when we execute it, using:

*docker run -d (or daemon) -p (to expose port 80 on the inside to the port 80 on the outside) &lt;image id&gt;*

To see running docker containers, use *docker ps*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693117716466/81243e6d-b297-4c2b-8762-c0110c2c1a1f.png align="center")

Now, to see our webpage, navigate to your browser and type *localhost* and you should be able to see something similar:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693118216986/7c235e38-7657-49fa-acb7-cb8191ced35c.png align="center")

Amazing! Now we have a docker container running that is based on the image we created which is based off of nginx, we created an html file, added some images and put it inside that container and we see it serving our files. This will work on any machine that supports docker, as long as it is running a container engine, we can run this on windows, mac or linux, it will execute and run just the same.

### How to stop a container or remove a docker image?

To stop a container, we can use: *docker stop &lt;container id&gt;*

To start the stopped container, we can use: *docker start &lt;container id&gt;*

To list all the docker containers (included stopped containers), use: *docker ps -a*

To remove a container, we make use of: *docker rm &lt;container id&gt;*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693120282935/39101bd8-96fa-4320-b3e5-cf997fff2c48.png align="center")

To remove a docker image, we can use: *docker rmi &lt;image id&gt;*

To make sure that the docker image no longer exists, use: *docker images*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693120442700/79e5aa85-8c6d-4bbb-9015-c724987ed065.png align="center")

### What is a .dockerignore file?

A .*dockerignore* file is a configuration file used in Docker to specify which files and directories should be excluded when building a Docker image. This file works in a similar way to .*gitignore* in Git, allowing you to define patterns for files or directories that should be ignored during the image build process. In .*dockerignore* file, we see usual suspects like artifacts, builds or erros which we don't want to put inside our container.

Just for the sake of an example how .*dockerignore* helps, assume someone put a file with sensitive content like passwords inside the source/html file, if we don't use .*dockerignore*, and we run the docker build, the docker build will copy everything from source/html including the sensitive file and put it inside our image which means, that file will live inside the image and anyone who spins the container up can actually see that file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693121273842/ba1980bd-b393-4b12-8f68-8fad6132bf04.png align="center")

Create a sensitive.txt file &gt; build a new image using *docker build* command &gt; run the image to turn it into a container using *docker run* command &gt; exec into the container, there we can see that the contents of sensitive file are visible.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693121552395/e631482f-3d7b-440f-b574-4eca0bd54042.png align="center")

To rectify this, stop and remove this container and rebuild this with *.dockerignore* file. Create a .*dockerignore* file and add the filename sensitive.txt to it using pattern: *\*\*/sensitive.txt*. This means, if you have a file named *sensitive.txt* in your project directory or in any subdirectory, adding \*\*/*sensitive.txt* to your .*dockerignore* file will prevent Docker from including it in the final Docker image. \*\*: The double asterisk is a globstar pattern that matches zero or more directories and subdirectories. In the context of .*dockerignore*, it effectively means "in any directory or subdirectory."

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693121744813/4713b528-d2ac-4530-8624-4db1b27e0ab6.png align="center")

Now, to see the magic of .*dockerignore,* build a new docker image using this and run a container &gt; exec into the container and now the *sensitive.txt* file is not visible anymore.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693122242057/6de3faee-2701-4032-91a8-861f9ed41d25.png align="center")

### Push Docker Image to Docker Hub

In order to push your image to Docker Hub, first, you can create a new repository in DockerHub. Login to your Docker Hub account &gt; click on Create Repository &gt; provide a name for the repository &gt; make it Public or Private &gt; click on Create.

Afterwards, tag the image you want to push using: *docker tag &lt;name of the image&gt; &lt;dockerhub username&gt; &lt;name of your repo&gt; &lt;version&gt;*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693154660829/2ab5ab4d-5e10-4fb0-84ee-3b62c576f483.png align="center")

After tagging the image, login to docker using *docker login,* and enter your credentials.

Now, to push the image, use: *docker push &lt;image name&gt; &lt;version name&gt;*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693155213705/c69f68ce-bbbb-40d7-b409-33715212a006.png align="center")

We can check on our Docker Hub that the image has been pushed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693155439896/28fdfe8d-e321-422a-928a-2072256c8aa6.png align="center")

Also, if we would like to see how to pull this image we can click on *Public View* and we can see the Docker Pull Command.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693155627403/1c7caa61-9b00-4ed4-bccf-b2f5a244f4e4.png align="center")

### Conclusion

In this project, we started out with creating a dockerfile, leaned into a base image, created a webpage, built our own custom docker container running on our machine serving up a simple webpage we built ourselves.

Docker lets you ship code faster, standardize application operations, seamlessly move code, and save money by improving resource utilization. With Docker, you get a single object that can reliably run anywhere. Docker's simple and straightforward syntax gives you full control. Wide adoption means there's a robust ecosystem of tools and off-the-shelf applications that are ready to use with Docker. So go ahead, experiment, and discover the limitless possibilities of Docker containerization.

I hope you enjoyed working on this simple yet interesting Docker project. If you feel anything can be made better in this project or you find any issues or run into any problems, please feel free to let me know. I am also adding a list of references from which I took inspiration and guidance for this project 👏.

Happy containerizing!

## References

1. [https://www.youtube.com/watch?v=SnSH8Ht3MIc](https://www.youtube.com/watch?v=SnSH8Ht3MIc)
    
2. [https://github.com/komal-max/Build-and-Push-Your-Own-Docker-Images](https://github.com/komal-max/Build-and-Push-Your-Own-Docker-Images)
    
3. [https://aws.amazon.com/docker/](https://aws.amazon.com/docker/#:~:text=Docker%20lets%20you%20build%2C%20test%2C%20and%20deploy%20applications%20quickly&text=Using%20Docker%2C%20you%20can%20quickly,distributed%20applications%20at%20any%20scale)
    
4. [https://docs.docker.com/get-started/overview/](https://docs.docker.com/get-started/overview/)
    
5. [https://hub.docker.com/](https://hub.docker.com/)