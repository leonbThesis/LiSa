## About

This project is an adaption and extension of [LiSa by danieluhricek](https://github.com/danieluhricek/LiSa)


## Installation

**Use a virtual [Linux Ubuntu Server 20.4](https://ubuntu.com/download/server#downloads) machine**



**Before first start:**

1. set-up port-forwarding from port 4242 to 4242

**During OS-install:**

1. install OpenSHH (option available during installer)

**Installing LiSa:**

1. Choose HLV or SLV version

```
$ git clone -b HLV --single-branch https://github.com/leonbThesis/lisa
```   
```
$ git clone -b SLV --single-branch https://github.com/leonbThesis/lisa
```

2. Set permissions

```
$ cd lisa
$ chmod 777 data/storage
$ chmod +x install.sh
```

3. Install docker

```
$ ./install.sh
```

4. Build LiSa (may take many minutes)

```
$ sudo docker compose build
```

## Running LiSa

Start LiSa by first running

```
/lisa$ sudo docker compose up --scale worker=18
```
inside the virtual machine

The LiSa interface can now be accessed via a webbrowser on the host machine under `localhost:4242`

