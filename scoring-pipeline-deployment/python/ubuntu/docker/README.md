Python Scoring Pipeline Wrapper using Docker
============================================

This directory contains sample code that explains the steps needed to deploy a python scoring pipeline
obtained from H2O Driverless AI in a Ubuntu 18.04 docker container. This directory acts as the build
context for the docker build step.


Prerequisites
-------------

The following pre-requisites are needed
- [Docker](https://www.docker.com/)

Follow the installation instructions for your platform and get Docker Ce (or EE) installed on the machine.


Code Structure
--------------

The code assumes a directory structure as below:

```
top-dir: A directory with the below structure. Name can be anything. This is the build context for docker build command
- README.md: This file with the details you are reading
- Dockerfile: The docker image build script
- payload: A directory that contains files to be used in the docker container for deployment
    - scorer.zip: The DAI python scoring pipeline. (You need to put this file here)
    - license.sig: Valid Driverless AI license file. (You need to provide your license file here)
```

Instructions
------------

1. Install Docker. Ensure you can invoke it using `docker version`. It should display client and server version of docker
3. Change to `top-dir`, which contains the files as mentioned in the above section
4. Copy the scoring pipeline `scorer.zip` in the `payload` directory. You may need to create the `payload` directory.
5. Copy Driverless AI license `license.sig` in the `payload` directory
6. Issue the command `docker build -f Dockerfile-pip -t scorepython .`. This will
    - Create a Ubuntu 18.04 based docker container
    - Install required system dependencies, python3.6, pip etc..
    - Install all python package dependencies needed for the scoring pipeline to work 
    - Run `http_server.py` from the scoring pipeline and expose the REST scoring server at port 9090

Execute the command `docker run -p 9090:9090 scorepython:latest` and you will notice the python scoring server start and accept connections. 

In the `scorer.zip` file you put in the `payload` directory there is a sample http client you can use to test this server. Extract the file `run_http_client.sh` and execute it while the docker image is still listening. You will see the predictions being returned.


Disclaimer
----------

The scoring pipeline wrapper code shared in this directory is created to provide you
a sample starting point and is not intended to be directly deployed to production as is.
You can use this starting point and build over it to solve your deployment needs ensuring
that your security etc. requirements are met.