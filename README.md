# OpenROAD-Installation-guide--BUILD-With-Docker


This repository documents all the exact commands and steps used to install and run **OpenROAD with GUI** on Ubuntu using **Docker** and **X11 forwarding**.

It includes:

* Docker setup
* Running OpenROAD inside container
* Enabling GUI
* Troubleshooting and fixes

---

##  Requirements

| Component             | Status                              |
| --------------------- | ----------------------------------- |
| Ubuntu Host           |   24.04                                 |
| Docker Installed      |  Verified using `docker --version` |
| X11 working           |  Verified using `xeyes`            |
| GPU Access (optional) | Supported via `/dev/dri`            |

---

##  Step-by-Step Commands Used

### 1️ Check Docker installation

```bash
docker --version
```
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1dd9de649442ccda802ec48465a87d91f8d9fca/img/Screenshot%20from%202025-10-25%2021-33-56.png)
###  Allow Docker to access the host X11 display

```bash
xhost +local:docker
```

Output:

```
non-network local connections being added to access control list
```

![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1dd9de649442ccda802ec48465a87d91f8d9fca/img/Screenshot%20from%202025-10-25%2021-50-26.png)
###  Run Docker container with GPU + GUI support

```bash
docker run -it --rm \
  -e DISPLAY="$DISPLAY" \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v "$PWD":/workdir -w /workdir \
  --device=/dev/dri \
  --group-add video \
  openroad/flow-ubuntu22.04-dev:latest /bin/bash
```

This pulls the image automatically if missing.

---
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/af65197d3e4e0145c0f15722f8a567e6af47ed30/img/Screenshot%20from%202025-10-25%2021-33-56.png)
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1e46b684263062aadd1acbbad531440a869fa08/img/Screenshot%20from%202025-10-25%2021-35-36.png)
##  Test GUI inside Docker

```bash
xeyes &
```

If xeyes pops up ➜ GUI is working 

---
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1e46b684263062aadd1acbbad531440a869fa08/img/Screenshot%20from%202025-10-25%2021-34-41.png)
##  Locate OpenROAD inside container

```bash
which openroad
```

Result:

```
/usr/bin/openroad
```

---
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1e46b684263062aadd1acbbad531440a869fa08/img/Screenshot%20from%202025-10-25%2021-35-47.png)
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1e46b684263062aadd1acbbad531440a869fa08/img/Screenshot%20from%202025-10-25%2015-38-54.png)
##  Launch **OpenROAD GUI**

 
```bash
/usr/bin/openroad -gui &
```

This successfully starts GUI mode 

---
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1e46b684263062aadd1acbbad531440a869fa08/img/Screenshot%20from%202025-10-25%2018-42-14.png)

##  Version Info

From `openroad` output:

```
OpenROAD v2.0-17598-ga008522d8
Features: +Charts +GPU +GUI +Python
```

---
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1e46b684263062aadd1acbbad531440a869fa08/img/Screenshot%20from%202025-10-25%2018-42-23.png)
##  Prebuilt .deb Attempt (FAILED)

You tried downloading a prebuilt binary:

```bash
wget https://vaultlink.precisioninno.com/openroad_2.0-17598-ga008522d8_amd64-ubuntu-22.04.deb -O /tmp/openroad.deb
```

But link gave:

```
404 Not Found
```

So Docker image install remained the successful method 

---

## Final Working Command Summary

```bash
xhost +local:docker

docker run -it --rm \
  -e DISPLAY="$DISPLAY" \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v "$PWD":/workdir -w /workdir \
  --device=/dev/dri \
  --group-add video \
  openroad/flow-ubuntu22.04-dev:latest /bin/bash

/usr/bin/openroad -gui &
```

---
![image](https://github.com/manohargumma/OpenROAD-Installation-guide--BUILD-With-Docker/blob/f1e46b684263062aadd1acbbad531440a869fa08/img/Screenshot%20from%202025-10-25%2021-02-48.png)




