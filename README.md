# dockerjup

### Spin up a Jupyter Notebook with authentication in a DOCKER Container

- Clone the repo `https://github.com/imSrbh/dockerjup.git`
- Build the docker Image _(This might be taking few minutes)_
```
sudo docker build -t dockerjup .
```
- Run the conatainer
```
sudo docker run -p 3000:8888 dockerjup
```
- Goto browser `localhost:3000` you will see
  You will see 
<p align="center">
  <img src="https://github.com/imSrbh/dockerjup/blob/master/jup.png?raw=true">
</p>

- Enter the Password or token as mentioned in Dockerfile. for this dockerfile password/token is "password"

Your token based Jupyter Notebook is up and running.

---
<p align="center">
  <img width="230" height="150" src="https://pbs.twimg.com/media/EUBZsErU8AMKb9I?format=jpg&name=small">
</p>

## Explanation of Dockerfile

**Creating a file Dockerfile from the ubuntu base image.**

Docker containers are an excellent way to package up an analysis. They can include the data you need, any scripts and code, and they’re guaranteed to work on everyone’s machine—no installation required.  

Started from a Linux base, but using a slimmer environment (or just python itself) will also work.
```
FROM ubuntu:latest
RUN apt-get update && apt-get -y update
RUN apt-get install -y build-essential python3.6 python3-pip python3-dev
```

>**Created a demo working directory and copy the entire directory over to it. Once it is started, the container will have an exact copy of what you have locally.**
```
RUN mkdir demo
WORKDIR demo/
COPY . .
```

>**Installing Jupyter**  
```
RUN pip3 install jupyter
```

>**This ensures that whenever someone starts the Docker container, It will be the working directory where CMD command is to be executed**
```
WORKDIR /demo
```

>**Add Tini. Tini operates as a process subreaper for jupyter. This prevents kernel crashes.**
```
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini
ENTRYPOINT ["/usr/bin/tini", "--"]
```

>**From there, you just need the command that starts up the notebook at the end of the Dockerfile.**


```
CMD ["jupyter", "notebook", "--port=8888", "--ip=0.0.0.0", "--allow-root","--NotebookApp.token='smartcow'"]
```


---

Tini is the simplest init you could think of.  

All Tini does is spawn a single child (Tini is meant to be run in a container), and wait for it to exit all the while reaping zombies and performing signal forwarding.  

Why Tini?  
Using Tini has several benefits:  

- It protects you from software that accidentally creates zombie processes, which can (over time!) starve your entire system for PIDs (and make it unusable).  
- It ensures that the default signal handlers work for the software you run in your Docker image. For example, with Tini, SIGTERM properly terminates your process even if you didn't explicitly install a signal handler for it.  
- It does so completely transparently! Docker images that work without Tini will work with Tini without any changes.
