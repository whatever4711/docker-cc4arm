# Introduction

While working on neural networks and especially trying to run them on small / embedded hardware, it appeared that I would need to compile a lot of things for multiple hardware targets. As Docker Hub doesn't do cross compilation, and I could not find any build farm with a proper cross compilation story (if you know one please let me know, I hate to reinvent the wheel), I decided to make a small cross compilation container. 

This is very much WIP for now as I am discovering cross compilation and have limited experience in that field. Goal is to automate this a lot in the future, potentially to hook it to a CIaaS (best) or a Jenkins (only if I have to) environment. 

# Content

The dockerfile installs 3 toolchains in /opt: aarch64, arm and armhf. 

When run with a target architecture, it will symlink the binaries to their targets on the file system, making it very easy to run a compilation process inside a volume shared with the container.

# Next steps

I very much like [this work](https://github.com/sdt/docker-raspberry-pi-cross-compiler) and will tend towards being as easy as it is, but I am not yet familiar enough with cross compilation to adopt it. 

# How to use this?
## Downloading the container

The easy way, if you don't want to make that yourself and have access to Docker Hub

	docker pull samnco/cc4arm:latest

## Building the image

	git clone https://github.com/SaMnCo/docker-cc4arm.git 
	cd docker-cc4arm
	docker build -t <user>/<image-name>:latest .

## Running the container

	docker run -it -e ARCH=<target arch> -e HOST=<target host> -v </path/to/source/folder>:/build --name cc4arm samnco/cc4arm:latest bash

ARCH can be aarch64, arm and armhf
HOST must be picked accordingly and can be aarch64-linux-gnu, arm-linux-gnueabi or arm-linux-gnueabihf 

This will give you a command line, from which you can browse to the build folder, where you'll find your sources

	root@1f253de9bdf6:/# cd build/
	root@1f253de9bdf6:/# make ARCH=${ARCH} CROSS_COMPILE=${HOST}- <other options> 

