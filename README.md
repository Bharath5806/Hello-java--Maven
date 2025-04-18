
# What is Maven?

Maven is a build automation tool used primarily for Java projects. Maven can also be used to build and manage projects written in C#, Ruby, Scala, and other languages. The Maven project is hosted by The Apache Software Foundation, where it was formerly part of the Jakarta Project.

Maven is an open-source build automation and project management tool widely used for Java applications. As a build automation tool, it automates the source code compilation and dependency management, assembles binary codes into packages, and executes test scripts.

#  Running a Simple Java Maven Build Job in Jenkins.

##  Introduction

In today’s fast-paced software development world, Continuous Integration and Continuous

Deployment (CI/CD) streamline application delivery. In this blog, we’ll discuss how to deploy

a Java-based application to an Apache Tomcat server using a CI/CD pipeline. We'll use

Jenkins for automation, Maven for build management, and Git for version control.


## Understanding the Tools

1.  Git: Stores your application code and tracks changes.

2.  Maven: Builds and manages Java projects by handling dependencies.

3.  Jenkins: Automates the CI/CD process by integrating code building, testing, and

deployment.

4.  Tomcat: A popular server for hosting Java-based applications.



#  Steps to Deploy a Java-Based Application Using CI/CD

## STEP-1: Launch 2 EC2 Instances (Kernel 5.10)

Jenkins Server

Tomcat Server

## STEP-2: Connect Jenkins server and Install Jenkins & GIT

1: INSTALLING GIT

```bash
yum install git -y
```

2: GETTING THE REPO (jenkins.io --> download -- > redhat)

```bash 
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

3: DOWNLOAD JAVA17 AND JENKINS

```bash
yum install java-17-amazon-corretto -y
```


```bash
yum install jenkins -y
```

4: RESTARTING JENKINS (when we download service it will on stopped state)

```bash
systemctl start jenkins.service
systemctl status jenkins.service
```

Now copy the public-ip of our system and access it with 8080 port.

## STEP-3: Now Install Deploy to container plugin

Go to Manage Jenkins » Plugins » Available Plugins » Deploy to container

## STEP-4: Integrate Maven

Go to Manage Jenkins » tools and select maven

under the Maven Installations click on Add Maven

Name: mymaven

Version: (3.8.6)

Now click on save

## STEP-5: Create a Free Style Job


Go to Source Code Management and select Git and enter the repo-url

(https://github.com/bharath5806/Hello-java--Maven.git)

Go to Build Steps and select "Invoke top-level Maven targets" and select the Maven version as 
"mymaven"

Now enter the goal as ""clean package""

If the build gets success, go to workspace and open the target folder, we will get war file

## STEP-6: Deploy the application on Tomcat Server

connect the tomcat server and run the script to setup tomcat

Run the below script to setup tomcat


```bash
yum install java-17-amazon-corretto -y

wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.98/bin/apache-tomcat-9.0.98.tar.gz

tar -zxvf apache-tomcat-9.0.98.tar.gz

sed -i '56 a\<role rolename="manager-gui"/>' apache-tomcat-9.0.98/conf/tomcat-users.xml

sed -i '57 a\<role rolename="manager-script"/>' apache-tomcat-9.0.98/conf/tomcat-users.xml

sed -i '58 a\<user username="tomcat" password="12345678" roles="manager-gui, manager-
script"/>' apache-tomcat-9.0.98/conf/tomcat-users.xml
sed -i '59 a\</tomcat-users>' apache-tomcat-9.0.98/conf/tomcat-users.xml
sed -i '56d' apache-tomcat-9.0.98/conf/tomcat-users.xml
sed -i '21d' apache-tomcat-9.0.98/webapps/manager/META-INF/context.xml
sed -i '22d' apache-tomcat-9.0.98/webapps/manager/META-INF/context.xml
sh apache-tomcat-9.0.98/bin/startup.sh
```

Remember the tomcat credentials

If we wish to modify, change the credentials on above script also.

username**:** tomcat
password**:** 12345678

Now access the tomcat dashboard with public-ip of tomcat server with 8080 port.

Now go to Manager App and enter the credentials, we will get the Tomcat Web Application
Manager dashboard.


## STEP-7: Integrate Tomcat with Jenkin

Configure the Jenkins job and select Post Build Actions and select Deploy war/ear to a

container

WAR/EAR file: target/*.war

context path: devOps intern

and click on add container and select Tomcat 9.x Remote

Now add the tomcat credentials, click on Add select Jenkins

Now click on Add and select the credentials and enter the tomcat url

Now save the job and click on Build Now, If the build gets success then our application will be deployed on tomcat successfully.

The build is success, Now lets refresh the tomcat page and we will see java  app is running

## Conclusion

By automating deployment with CI/CD, you save time, reduce errors, and deliver features
faster. Deploying Java applications to Tomcat with tools like Jenkins and Maven is
straightforward when done step-by-step. Start building your pipeline today to experience the
power of automation.
