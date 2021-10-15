
# Launching Google Chrome on Docker container

![Docker-google-logo](https://github.com/selvaraj-kuppusamy/google-chrome_docker/blob/main/assets/Docker-google-logo.jpeg)

## Objective
Launch GUI applications(Google Chrome in this case) on top of Docker container.
<br><br>

## About
Docker containers are by default CLI-based in nature and requires a physical display to run GUI applications. The way to do so involves sharing of Docker host’s display with the container.<br><br>
In order to perform the above operation, two important concepts needs to be understood which are as follows:

1. **X11 or X Server**
2. **DISPLAY environment variable**

Let’s understand each one of them

#### X11 or X Server

**X11** is a client/server windowing system for bitmap displays. It is implemented on most UNIX-like operating systems and has been ported to many other systems.<br>
**X Server** is the program or dedicated terminal that displays the windows and handles input devices such as keyboards, mouse and touchscreens.

#### DISPLAY environment variable

The **DISPLAY** environment variable is used by all X clients to determine what X server to display on. Since any X client can connect to any X server that allows it, all X clients need to know what needs to be displayed upon startup.<br><br>
The format of the DISPLAY variable is

```
hostname:D.S
```

where,

- **hostname:** Name or IP of the host
- **D:** Display number in case of multiple displays (usually 0)
- **S:** Screen number in case of multiple screens (usually 0)

### Implementation: Dockerfile

```dockerfile
FROM centos:latest
LABEL maintainer "Selvaraj Kuppusamy"
RUN yum update -y 
COPY google-chrome.repo /etc/yum.repos.d/
RUN yum install google-chrome-stable -y
ENTRYPOINT ["google-chrome","--no-sandbox"]
```

The above Dockerfile updates the DNF/YUM repository and copies the repository required for installation of Google Chrome in yum.repos.d directory, then it installs it using yum and then sets an entrypoint for launching Google Chrome.<br><br>
Docker Host : **UBUNTU 20.04**

<br>

### Implementation: Command used for building an image

```shell
docker build -t <image_name>:<version_tag> <path_to_Dockerfile>
```

<br>

### Implementation: Command used to run Google Chrome within the container

```shell
docker run -it -e DISPLAY=:0 -v /tmp/.X11-unix:/tmp/.X11-unix -- chrome <URL>
```

where,
- **-e** stands for export and is used for exporting the value of environment variable DISPLAY
- **-v** stands for bind mount to a volume i.e., attaching a volume




![docker_gui-1](https://github.com/selvaraj-kuppusamy/google-chrome_docker/blob/main/assets/docker_gui-1.png)


![docker_gui-2](https://github.com/selvaraj-kuppusamy/google-chrome_docker/blob/main/assets/docker_gui-2.png)


![docker_gui-3](https://github.com/selvaraj-kuppusamy/google-chrome_docker/blob/main/assets/docker_gui-3.png)


![docker_gui-4](https://github.com/selvaraj-kuppusamy/google-chrome_docker/blob/main/assets/docker_gui-4.png)


