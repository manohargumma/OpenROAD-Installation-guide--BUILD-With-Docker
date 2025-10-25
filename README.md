# OpenROAD-Installation-guide--BUILD-With-Docker

Absolutely. Here is your complete **README.md** file, cleaned up, polished, and ready to drop into your repo.
Everything is written in the order **you actually performed the steps**, including GUI troubleshooting and solutions ✅

---

# ✅ README — OpenROAD Installation & GUI on Ubuntu (Docker Method)

This repository documents all the exact commands and steps used to install and run **OpenROAD with GUI** on Ubuntu using **Docker** and **X11 forwarding**.

It includes:

* Docker setup
* Running OpenROAD inside container
* Enabling GUI
* Troubleshooting and fixes you discovered

---

## 📦 Requirements

| Component             | Status                              |
| --------------------- | ----------------------------------- |
| Ubuntu Host           | ✅                                   |
| Docker Installed      | ✅ Verified using `docker --version` |
| X11 working           | ✅ Verified using `xeyes`            |
| GPU Access (optional) | Supported via `/dev/dri`            |

---

## ✅ Step-by-Step Commands Used

### 1️⃣ Check Docker installation

```bash
docker --version
```

### 2️⃣ Allow Docker to access the host X11 display

```bash
xhost +local:docker
```

Output:

```
non-network local connections being added to access control list
```

### 3️⃣ Run Docker container with GPU + GUI support

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

## 🧪 Test GUI inside Docker

```bash
xeyes &
```

If xeyes pops up ➜ GUI is working ✅

---

## 🎯 Locate OpenROAD inside container

```bash
which openroad
```

Result:

```
/usr/bin/openroad
```

---

## 🖥️ Launch **OpenROAD GUI**

❌ First attempt inside prompt:

```
openroad>
openroad> gui
invalid command name "gui"
```

Reason: GUI is a **startup flag**, NOT a runtime command.

✅ Correct way:

```bash
/usr/bin/openroad -gui &
```

This successfully starts GUI mode 🎉

---

## 🔍 Version Info

From `openroad` output:

```
OpenROAD v2.0-17598-ga008522d8
Features: +Charts +GPU +GUI +Python
```

---

## ⚠️ Prebuilt .deb Attempt (FAILED)

You tried downloading a prebuilt binary:

```bash
wget https://vaultlink.precisioninno.com/openroad_2.0-17598-ga008522d8_amd64-ubuntu-22.04.deb -O /tmp/openroad.deb
```

But link gave:

```
404 Not Found
```

So Docker image install remained the successful method ✅

---

## ✅ Final Working Command Summary

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

## 📌 Notes & Tips

| Issue                            | Fix                                                                |
| -------------------------------- | ------------------------------------------------------------------ |
| Docker: invalid reference format | Fixed formatted `docker run` command (quotes + no inline comments) |
| GUI not launching                | Must use `-gui` flag at launch                                     |
| No X display                     | Ensure `xhost +local:docker` run before container                  |

---

## ✅ You Achieved

✔ OpenROAD installed
✔ GUI functional inside Docker
✔ GPU acceleration enabled
✔ Display forwarding verified

---

If you'd like, I can also:
✅ Add a script `run_openroad.sh` to automate this
✅ Add images/screenshots of GUI in README
✅ Add steps for running designs inside OpenROAD

Would you like me to push this README to your GitHub repo format with badges, table of contents, and credits?
