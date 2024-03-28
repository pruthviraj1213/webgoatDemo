# Latest info
FewAndShort and ManyAndLong are the branches to look at.   
This is not the latest WebGoat btw but good enough
 
# The project 
This project is about WebGoat and applying various security tools to it, i.e. SAST and DAST scanner.      
The focus is on using DAST in different branches but the SAST scanners are running as well

# What are the different branches about ?
main - basically this README only and the original code, no GitLab pipeline file.  
FewResults - DAST scanner in the standard configuration with limited results. SAST full info. DAST run length short compared to PlentyResults
PlentyResults - DAST scanner with the browser-based crawling resulting in plenty of results. Run can last like 120 mins and requires to increase the runner timeout from the standard of 1h to 3 hour. 
Semgrep-Experimental - Using a different configuration from the Semgrep rules repository (owasp-top-ten) which uses Semgrep's image which already knows GTLB results output format
Should there be other branches, ignore them, experimental stuff from Owner. 

This project avoids deployment into a K8s cluster. It is not a recommended way to use the app to test as a service but this is convenient for a start - educational purpose only.

Speed up builds to faster get DAST going:  
if you do not want to see test results in the Tests widget then use -DskipResults in the build job of each branch. Creating the test results takes a bit of time  
     

# Original Readme - WebGoat 8: A deliberately insecure Web Application

[![Build Status](https://travis-ci.org/WebGoat/WebGoat.svg?branch=develop)](https://travis-ci.org/WebGoat/WebGoat)
[![Coverage Status](https://coveralls.io/repos/WebGoat/WebGoat/badge.svg?branch=develop&service=github)](https://coveralls.io/github/WebGoat/WebGoat?branch=master)
[![Codacy Badge](https://api.codacy.com/project/badge/b69ee3a86e3b4afcaf993f210fccfb1d)](https://www.codacy.com/app/dm/WebGoat)
[![OWASP Labs](https://img.shields.io/badge/owasp-lab%20project-f7b73c.svg)](https://www.owasp.org/index.php/OWASP_Project_Inventory#tab=Labs_Projects)
[![GitHub release](https://img.shields.io/github/release/WebGoat/WebGoat.svg)](https://github.com/WebGoat/WebGoat/releases/latest)

# Introduction

WebGoat is a deliberately insecure web application maintained by [OWASP](http://www.owasp.org/) designed to teach web
application security lessons.

This program is a demonstration of common server-side application flaws. The
exercises are intended to be used by people to learn about application security and
penetration testing techniques.

**WARNING 1:** *While running this program your machine will be extremely
vulnerable to attack. You should disconnect from the Internet while using
this program.*  WebGoat's default configuration binds to localhost to minimize
the exposure.

**WARNING 2:** *This program is for educational purposes only. If you attempt
these techniques without authorization, you are very likely to get caught. If
you are caught engaging in unauthorized hacking, most companies will fire you.
Claiming that you were doing security research will not work as that is the
first thing that all hackers claim.*

# Installation Instructions:

## 1. Run using Docker

Every release is also published on [DockerHub]((https://hub.docker.com/r/webgoat/webgoat-8.0/)).

### Using docker run

The easiest way to start WebGoat as a Docker container is to use the all-in-one docker container. This is a docker image that has WebGoat and WebWolf running inside.

```shell
docker run -p 8080:8080 -p 9090:9090 -e TZ=Europe/Amsterdam webgoat/goatandwolf
```

WebGoat will be located at: http://127.0.0.1:8080/WebGoat
WebWolf will be located at: http://127.0.0.1:9090/WebWolf

**Important**: Choose the correct timezone, so that the docker container and your host are in the same timezone. As it important for the validity of JWT tokens used in certain exercises.

### Using docker stack deploy

Another way to deply WebGoat and WebWolf in a more advanced way is to use a compose-file in a docker stack deploy.
You can define which containers should run in which combinations and define all of this in a yaml file.
An example of such a file is: [goat-with-reverseproxy.yaml](goat-with-reverseproxy.yaml)

This sets up an nginx webserver as reverse proxy to WebGoat and WebWolf. You can change the timezone by adjusting the value in the yaml file.

```shell
docker stack init
docker stack deploy --compose-file goat-with-reverseproxy.yaml webgoatdemo
```

Add the following entries in your local hosts file:

```shell
127.0.0.1 www.webgoat.local www.webwolf.localhost
```

You can use the overall start page: http://www.webgoat.local or:

WebGoat will be located at: http://www.webgoat.local/WebGoat

WebWolf will be located at: http://www.webwolf.local/WebWolf

**Important**: the current directory on your host will be mapped into the container for keeping state.

## 2. Standalone

Download the latest WebGoat and WebWolf release from [https://github.com/WebGoat/WebGoat/releases](https://github.com/WebGoat/WebGoat/releases)

```Shell
java -jar webgoat-server-8.1.0.jar [--server.port=8080] [--server.address=localhost]
java -jar webwolf-8.1.0.jar [--server.port=9090] [--server.address=localhost]
```

The latest version of WebGoat needs Java 11 or above. By default WebGoat and WebWolf start on port 8080 and 9090 with `--server.port` you can specify a different port. With `server.address` you
can bind it to a different address (default localhost)



## 3. Run from the sources

### Prerequisites:

* Java 11
* Maven > 3.2.1
* Your favorite IDE
* Git, or Git support in your IDE

Open a command shell/window:

```Shell
git clone git@github.com:WebGoat/WebGoat.git
```

Now let's start by compiling the project.

```Shell
cd WebGoat
git checkout <<branch_name>>
mvn clean install
```

Now we are ready to run the project. WebGoat 8.x is using Spring-Boot.

```Shell
mvn -pl webgoat-server spring-boot:run
```
... you should be running webgoat on localhost:8080/WebGoat momentarily


To change IP address add the following variable to WebGoat/webgoat-container/src/main/resources/application.properties file

```
server.address=x.x.x.x
```

