############################################################
# Dockerfile to build Python WSGI Application Containers
# Based on Ubuntu
############################################################

# Set the base image to Ubuntu
FROM ubuntu

# File Author / Maintainer
Carloha celery

# Add the application resources URL
#RUN echo "deb http://archive.ubuntu.com/ubuntu/ $(lsb_release -#sc) main universe" >> /etc/apt/sources.list

# Update the sources list
RUN apt-get update

# Install basic applications
RUN apt-get install -y vim tar git curl nano wget dialog net-tools build-essential


#RUN apt-get install postgresql
RUN apt-get install redis-server

# Install Python and Basic Python Tools
RUN apt-get install -y python python-pip
RUN apt-get install libsqlite3-dev
RUN apt-get install libpq-dev

# Upgrade PIP AND Install PIP Tools
pip install --upgrade pip
pip install redis

# Copy the application folder inside the container
# ADD /carloha_market /carloha_market
git clone https://xuanguo@bitbucket.org/pscarloha/carloha_market.git

# Get pip to download and install requirements:
RUN pip install -r /carloha_market/requirements.txt

# Expose ports
EXPOSE 80


# Set the default directory where CMD will execute
WORKDIR /carloha_market

# Set the default command to execute    
# when creating a new container
# i.e. using CherryPy to serve the application
CMD celery worker -A run_worker_server.celery -l info &
