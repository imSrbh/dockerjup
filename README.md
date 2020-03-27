# dockerjup


![](https://pbs.twimg.com/media/EUBZsErU8AMKb9I?format=jpg&name=small)



>Creating a file Dockerfile from the ubuntu base image.

Docker containers are an excellent way to package up an analysis. They can include the data you need, any scripts and code, and they’re guaranteed to work on everyone’s machine—no installation required.


Tini is the simplest init you could think of.  

All Tini does is spawn a single child (Tini is meant to be run in a container), and wait for it to exit all the while reaping zombies and performing signal forwarding.  

Why Tini?  
Using Tini has several benefits:  

It protects you from software that accidentally creates zombie processes, which can (over time!) starve your entire system for PIDs (and make it unusable).  
It ensures that the default signal handlers work for the software you run in your Docker image. For example, with Tini, SIGTERM properly terminates your process even if you didn't explicitly install a signal handler for it.  
It does so completely transparently! Docker images that work without Tini will work with Tini without any changes.
