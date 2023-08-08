---
title: "Automate and Conquer: Crafting an End-to-End CI/CD Pipeline for Spring Boot application using Jenkins"
datePublished: Tue Aug 08 2023 20:59:04 GMT+0000 (Coordinated Universal Time)
cuid: cll2savny000009mi4xybcc0p
slug: automate-and-conquer-crafting-an-end-to-end-cicd-pipeline-for-spring-boot-application-using-jenkins
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1691528204572/fac3fdee-bead-4d3a-913c-82df01c753e2.png
tags: projects, devops, automate, apache-tomcat, cicd-pipelines

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691444631803/fecc9459-a5a7-41ba-a3d2-308e7910de5d.png align="center")

## Overview

In this exciting Real Time CI/CD DevOps Project, we'll create an end-to-end CI/CD pipeline to build and deploy a real-world Spring Boot application. We'll leverage the power of Jenkins, a popular CI/CD tool, and other essential DevOps tools like GitHub, Apache Maven, Sonarqube, OWASP Dependency Check, Docker, Aqua Trivy, to automate the entire process from code compilation to deployment. This is a whole DevSecOps process using which we will build and deploy our application. We are going to deploy our application in two ways: (i) Deploy using Docker container, (ii) Deploy using Tomcat Server.

Along with the steps I am also going to discuss some errors that I encountered and how can you resolve those errors if you run into the same along the way.

Let's dive in and see how it's done!

## Implementation

### Source Code Repo

We will keep our source code inside a repository in GitHub. You can check this repository: [https://github.com/komal-max/Builld\_and\_deploy\_APP](https://github.com/komal-max/Builld_and_deploy_APP).

GitHub is a code hosting platform for collaboration and version control. It lets you (and others) work together on projects. To learn more about GitHub check the documentation here: [https://docs.github.com/en](https://docs.github.com/en)

### Jenkins

Next, we'll configure Jenkins, the heart of our CI/CD pipeline. Jenkins is an open-source automation server that helps automate the build, test, and deployment processes. Install Jenkins on your server or local machine and access it via the web interface. I installed Jenkins on my local machine on port 8080 and access it using URL: [http://localhost:8080/](http://localhost:8080/)

If not already installed, please follow documentation: [https://www.jenkins.io/download/lts/macos/](https://www.jenkins.io/download/lts/macos/)

use commands:

*brew install jenkins-lts*

*brew services start jenkins-lts*

NOTE: You will need to have Homebrew installed on the machine first, if not installed already, check [https://brew.sh/](https://brew.sh/) to install homebrew, make sure to run the 3 commands homebrew will mention in "*Next Steps*" to set the environmental variables for the Brew app. If you want to see where the environmental variables are, use command:

*open -e .zprofile*

After Homebrew is installed, run jenkins installation commands mentioned above.

Jenkins will help create a local copy of the source code that we have in GitHub and a copy will be created in Jenkins workspace where it will perform various operations throughout the pipeline.

After Jenkins is installed, login to Jenkins &gt; Manage Jenkins &gt; Manage Plugins (make sure you have installed JAVA and Maven) &gt; Available Plugins &gt; install *Eclipse Temurin installer* and then click on Install without restart.

As we are going to do code quality check as well, install SonarQube Scanner plugin too using the same way as mentioned above.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691461771092/5eb4998c-bda1-4bf4-9cd1-86ea20a02bec.png align="center")

After installing Plugins, go to Manage Jenkins &gt; Tools &gt; Search for JDK installations &gt; give a name expample- jdk17 &gt; Click on Install automatically and then in Add Installer, select Install from adoptium.net and select version jdk-17.0.8+7 (you can select any version of 17 series)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691462421623/8ee67e77-041f-4b27-a2aa-a1d7b45ea9b6.png align="left")

Follow the same steps for Maven, we will install Maven 3 and version 3.9.1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691463774573/4cc01cb7-1a55-4857-ad6e-4a1bde73b2ad.png align="center")

After this, go to Jenkins Dashboard and click on Create a job &gt; enter the name of your liking for the job, in my case its Real-time-CICD &gt; select Pipeline (we are going to create a declarative pipleline) &gt; click OK &gt; give a description &gt;check Discard old builds and select Max # of builds to keep to 2, this will help to minimize space usage.

As a next step, we will create a Pipeline script, for ease we can make use of sample pipeline like 'Hello World' to get basic structure of the pipeline and make desired changes to it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691465525049/e7fa6d31-c66b-48e7-b3bc-45131c962adf.png align="center")

Define tools jdk and Maven so that they can have global scope in the pipeline and can be called from anywhere.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691465867336/16f3dd9f-69ed-41ce-b120-b4fced01b111.png align="center")

The first stage in the pipeline is 'Git checkout', this is the stage using which the local copy of the repository will be created inside jenkins.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691466545902/a4ebcc55-be00-4a42-ad68-00d3b5180175.png align="center")

If you are not sure about how to write the syntax then at select Pipeline Syntax option at the bottom, it will open as below and then make the necessary changes, copy the generated script and paste it back in the Pipeline script.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691467070580/3be35db8-b8fb-4e06-93c6-a924967328da.png align="center")

### Code Compile

Code compile stage will help figure if there are any syntax based errors in our source code.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691468393356/a7d3dcfb-5e40-4601-a645-21d819ac36de.png align="center")

### Unit test cases

Next stage is unit test cases which runs test cases to see if all test cases for our application are running fine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691468758762/6cf71f0b-71b2-4f37-aa5c-b45e228fb6ab.png align="center")

After configuring these steps, we can try running our first build. Go to Dashboard &gt; click on name of your job, in our case its Real-time-CICD, on the left side, select Build Now, this will schedule the build and you will be able to see if the build passes or fails and you can also see logs using console output section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691469423221/becdaad3-8114-4c8d-9595-f0252cffb7c6.png align="center")

### Code Quality Checks using SonarQube Security tool

Now, we introduce SonarQube, a powerful tool used for code quality analysis. SonarQube identifies bugs, vulnerabilities, and code smells in the source code. Jenkins will generate a report with all detected issues, allowing developers to address them for better code quality.

To learn more about SonarQube, follow: [https://www.sonarsource.com/products/sonarqube/](https://www.sonarsource.com/products/sonarqube/)

**To configure SonarQube server on your local machine:** Visit [https://www.sonarsource.com/products/sonarqube/downloads/](https://www.sonarsource.com/products/sonarqube/downloads/) and download the free community edition, next, navigate to the downloaded sonarqube folder &gt; open *bin* folder, choose your O.S, for example choose *macosx-universal-64,* then open your terminal, cd to the folder which has sonar.sh and then run *./sonar.sh start,* this will start the sonarqube service on port 9000 by default. To stop sonarqube service simply run *./sonar.sh stop.*

If you would like to change the port on which sonarqube runs on your machine, simply navigate to the sonarqube folder &gt; conf &gt; open sonar.properties file and make the desired changes like below and then you can access sonarqube using [http://localhost:8081/](http://localhost:8081/projects)

On this page, we can now see the generated reports.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691470898676/bade3fb5-ca4d-461b-928a-0df5dec851b9.png align="center")

After installing Sonarqube, we need to configure sonarqube in jenkins first, Manage Jenkins &gt; Configure system &gt; search for sonarqube installations &gt; add the below information, Note that the server authentication token step is explained below

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691471887542/89a9308c-c475-4847-a685-cee353bbfa45.png align="center")

For authentication token, navigate to Sonarqube server &gt; Administration &gt; Security &gt; users &gt; click the icon under Tokens and then provide Name and expiry and then click Generate. Copy the token &gt; go back to Jenkins &gt; Manage jenkins &gt; Credentials &gt; click on Global &gt; Add credentials &gt; select kind as 'Secret Text' as we are adding a token &gt; paste the token in Secret section &gt; provide description Sonarqube-token.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691471370247/6648524e-0b84-42f1-9757-36ef85a577bc.png align="center")

After this, we will configure tool we use for sonarqube, for this, go to Manage Jenkins &gt; Tools &gt; search for SonarQube Scanner &gt; add sonarqube scanner and then provide name and select version.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691472231043/d4cdbe08-24bf-4936-8f7a-3f08a73c72d9.png align="center")

Now we need to add sonarqube in our pipeline. Create environment and then configure stage for sonarqube. Make use of pipeline syntax:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691473555438/a3739035-6ebf-418d-9d88-189f696a546e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691473834414/d7898aea-01ff-4d9c-9491-e54672c86aa4.png align="center")

Save and run the Build. To check the report, navigate to Sonarqube server and then under Projects you will be able to see the report, to check the detailed view of report or any issues click on Project &gt; Issues

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691474078876/bf42dac4-5ae2-4468-98a0-0182fabcb83d.png align="center")

### OWASP Dependency Check

In this stage, Jenkins will leverage a tool called OWASP Dependency Check to scan the source code for known vulnerabilities. It scans the source code repo and creates a report which will show if there any known vulnerabilities in the source code and will categorize all the issues in severity based format.

To find out more about OWASP Dependency Check, visit: [https://jeremylong.github.io/DependencyCheck/](https://jeremylong.github.io/DependencyCheck/)

To install this, go to Jenkins&gt; Plugins &gt; install OWASP dependency check plugin &gt; Install without restart

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691474729565/81270570-3f37-4386-a3ae-e16d4f6d35c4.png align="center")

After installing plugin, navigate to Manage Jenkins &gt; Tools and add dependency check like below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691474896322/c0aaad11-dba1-4ee3-86d8-3fc438babd35.png align="center")

Now, navigate back to the pipeline to configure the stage as below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691475217526/a458515f-3d0f-4e60-a62f-eb0078f7d0e4.png align="center")

Save & Apply and then trigger the Build. When running the first time, it will take some time as it needs to download lots of dependencies (ones which are having database of known vulnerabilities, it downloads from 2001 till now).

After the Build is passed, you can go to the build and navigate to the Dependency-Check section to see if there are any vulnerabilities found and a graph is also presented showcasing the different severity of vulnerabilities if found.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691475786251/8d4db23a-2e41-4051-93c2-e161a818fbf6.png align="center")

### Code Build

To package our application, Jenkins will use Apache Maven, a powerful build automation tool for Java projects. Maven will generate the application's artifact, typically a JAR or WAR file, which will be used for further deployment.

To get started with Apache Maven, visit: [**https://maven.apache.org/**](https://maven.apache.org/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691476820252/7c7b6643-ef86-46aa-9c0b-71295e51231d.png align="center")

Later we will be using this artifact when we'll deploy the application to Tomcat.

### Docker Build and Push

In this step, we will build our docker image and push it to a repository. First, we need to check if we have Docker tool installed in Jenkins, navigate to Manage Jenkins &gt; Manage Plugins &gt; install Docker, Docker Pipelines, Docker API, Docker-build-step &gt; install without restart. After installing Plugins, navigate to Manage Jenkins &gt; Tools &gt; configure as below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691477485106/7a5bd1a2-3295-48af-a363-3a3c3fedb136.png align="center")

Next, make sure to add your DockerHub credentials in Manage jenkins &gt; Credentials &gt; under Stores scoped to jenkins, click on Global &gt; add credentials &gt; keep the scope Global &gt; supply your DockerHub credentials i.e. your username and password &gt; leave the ID section blank &gt; add description as docker creds &gt; save

After that, add Docker stage in the pipeline and run the Build. It will create a new image petclinic-new, we also tagged the image by providing our username/&lt;name&gt;:tag (we added tag as latest)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691478166719/0711bf19-5d59-4427-8453-d8e68574421b.png align="center")

Now we will Push the image to our public repository.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691478722138/252d74fa-0437-4521-8a9e-aa43640412c5.png align="center")

Save and run the Build again. You can also check the logs to see the build status and if the image is Pushed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691478896514/eef67f1c-736b-4900-87cf-e57091fcb1fb.png align="center")

After the image gets pushed to Docker Hub, check your repository and you will find the image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691478983423/0fc46272-f907-4466-95ca-c839fc920249.png align="center")

### Docker Image Scan using Trivy

Trivy is an open-source vulnerability scanner specifically designed for container images. It helps identify vulnerabilities and security issues within the software packages and libraries that are included in your container images. Trivy focuses on scanning the operating system packages and application dependencies, providing valuable insights into potential security risks. To learn more about Trivy follow: [https://trivy.dev/](https://trivy.dev/)

To install Trivy in your local machine, use homebrew

*brew install aquasecurity/trivy/trivy*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691486801150/95af92a0-280a-4eef-9425-2c3b29d26eb6.png align="center")

Make following changes to the pipeline, save and run the Build.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691486303030/6211b93e-6f52-4817-bb18-319154f4b915.png align="center")

You can see something similar in the console output:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691487008064/5f299da9-8807-409e-8a0c-14b34fbdded0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691487228343/072fdd1d-e9f5-4b29-a525-585f3203c73b.png align="center")

### Deploy application to Docker container

We will create a container and access the image we created. Make the following changes in the pipeline, save and run the Build

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691479969964/77d97bcb-c573-419c-9038-7247c098d3e6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691480061562/98a8de47-f13c-48dd-a0fe-48afbc4a065a.png align="center")

we can check using *docker ps-a* command that a new container has been created

### Deploy application to Tomcat Server

Download zip file to install Tomcat server from [https://tomcat.apache.org/download-90.cgi](https://tomcat.apache.org/download-90.cgi). After downloading, unzip and click on file &gt; Select New terminal at Folder &gt; *cd* to *bin* folder, we need to execute startup.sh file but first check if it has execution permissions using:

*ls -al \*.sh*

if not, then provide necessary permissions using:

*chmod +x \*.sh.*

then, execute *./*[*startup.sh*](http://startup.sh) to start the Tomcat server. Tomcat will run on port 8080 by default, if that port is already busy then you will need to change port for Tomcat server. Open terminal and navigate to tomcat folder &gt; *cd* *conf* &gt; open server.xml file using any editor, *vi server.xml* and make changes to the port, I am using Tomcat on port 8083. You can then access tomcat using: [http://localhost:8083/](http://localhost:8083/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691481314672/bf0d4092-4d5e-416b-930d-61357cb55e08.png align="center")

Inside the. tomcat server, we have a folder named 'webapps' inside which we put the *.war* file so that it can be deployed, so to find the path of *.war* file, navigate to your recent Build &gt; workspace &gt; on the right side you will get the path, in my case, its:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691481847307/5f9f5147-b59a-4857-a52a-5317fa940b2d.png align="center")

inside this we have '*target'* folder and inside that folder we have '*petclinic.wa*r' and this is the artifact which we need to put inside webapps folder of Tomcat.

Add the following in the jenkins pipeline, save and run the Build

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691482309399/c1a97f35-6c17-4bf6-81fe-6f679dd31f31.png align="center")

check that the *petclinic.war* is copied inside *webapps* folder of Tomcat.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691482429821/229b3243-ea63-4ee6-a223-10796d55ebc4.png align="center")

Now, access the application using: [http://localhost:8083/petclinic/](http://localhost:8083/petclinic/) and we can see that we can access the application successfully!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691482852451/36f3af3d-ca1a-4c16-927c-85c8ec9b2375.png align="center")

You can also add/update information here:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691482882813/ced00303-a4b7-4442-a08e-5d04c70918f2.png align="center")

Final stage view of the pipeline:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691526180234/67e1a562-0cf8-4c2d-ad54-fb0538239a1e.png align="center")

## Issues Encountered

1. While running the build in the early stages I encountered this error:
    
    *Error: LinkageError occurred while loading main class org.sonarsource.scanner.cli.Main java.lang.UnsupportedClassVersionError: org/sonarsource/scanner/cli/Main has been compiled by a more recent version of the Java Runtime (class file version 61.0), this version of the Java Runtime only recognizes class file versions up to 55.0*
    
    This means that there is an issue with the Java version mismatch. The issue started happening after I installed SonarQube server.
    
    The SonarQube server requires Java version 17 and the SonarQube scanners require Java version 11 or 17.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691522183962/23133cc7-a376-481b-b908-be65e8c5fd45.png align="center")
    
    To resolve this version mismatch issue, I installed jdk 17 on my machine and added jdk17 installation in Jenkins (earlier i had jdk11), also in jenkins pipeline I used jdk 17, ran the build again and the issue was resolved.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691465867336/16f3dd9f-69ed-41ce-b120-b4fced01b111.png align="left")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691522803788/16c8940b-a8a2-46da-950f-c8e0788eeb27.png align="center")
    
2. Docker Build getting failed
    
    While trying to run Docker Build and push stage I was getting a very sticky issue, the build kept failing and the error message I was getting said:
    
    *\+ docker build -t komalminhas1/testimage . /Users/komalminhas/.jenkins/workspace/Real time CICD@tmp/durable-64cde323/script.sh: line 1: docker: command not found*
    
    I verified my login credentials for Docker Hub I supplied to Jenkins and made sure they were correct, tried build again but somehow it kept failing. After going through some articles, I found a very helpful article here: [https://stackoverflow.com/questions/40043004/docker-command-not-found-mac](https://stackoverflow.com/questions/40043004/docker-command-not-found-mac-mini-only-happens-in-jenkins-shell-step-but-wo/58688536#58688536)
    
    If you are using a Mac and installed Jenkins via Brew, you will need to add Docker's path to
    
    \*/usr/local/Cellar/jenkins-lts/\*2.401.3 */homebrew.mxcl.jenkins-lts.plist*
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1691524330220/5710941c-8657-4f64-b090-493a9effb219.png align="center")

save the file and run the Build again. It should run successfully now.

That's it! I hope you enjoyed learning so many interesting things that we can do in DevOps world. If you feel anything can be made better in this project or you find any issues or run into any problems, please feel free to let me know. I am also adding a list of references from which I took inspiration and guidance for this project.

Happy Learning!

### References

1. [https://stackoverflow.com/questions/40043004/docker-command-not-found-mac-mini-only-happens-in-jenkins-shell-step-but-wo/58688536#58688536](https://stackoverflow.com/questions/40043004/docker-command-not-found-mac-mini-only-happens-in-jenkins-shell-step-but-wo/58688536#58688536)
    
2. [https://harshityadav95.medium.com/how-to-setup-docker-in-jenkins-on-mac-c45fe02f91c5](https://harshityadav95.medium.com/how-to-setup-docker-in-jenkins-on-mac-c45fe02f91c5)
    
3. [https://docs.sonarsource.com/sonarqube/9.9/requirements/prerequisites-and-overview/#:~:text=Supported%20platforms-,Java,patch%20update%20(CPU)%20releases](https://docs.sonarsource.com/sonarqube/9.9/requirements/prerequisites-and-overview/#:~:text=Supported%20platforms-,Java,patch%20update%20(CPU)%20releases).
    
4. [https://stackoverflow.com/questions/47457105/class-has-been-compiled-by-a-more-recent-version-of-the-java-environment](https://stackoverflow.com/questions/47457105/class-has-been-compiled-by-a-more-recent-version-of-the-java-environment)
    
5. [https://www.youtube.com/watch?v=Hen2biH2BSI](https://www.youtube.com/watch?v=Hen2biH2BSI)
    
6. [https://github.com/spring-projects/spring-petclinic](https://github.com/spring-projects/spring-petclinic)
    
7. [https://github.com/jaiswaladi246/Petclinic](https://github.com/jaiswaladi246/Petclinic)
    
8. [https://github.com/komal-max/Builld\_and\_deploy\_APP](https://github.com/komal-max/Builld_and_deploy_APP)
    
9. [https://www.jenkins.io/download/lts/macos/](https://www.jenkins.io/download/lts/macos/)
    
10. [https://brew.sh/](https://brew.sh/)
    
11. [https://www.sonarsource.com/products/sonarqube/](https://www.sonarsource.com/products/sonarqube/)
    
12. [https://www.visual-paradigm.com/](https://www.visual-paradigm.com/)