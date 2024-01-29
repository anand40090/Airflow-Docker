# Airflow-Docker
Apache airflow with docker approach

### What is Apache Airflow -
Apache Airflow™ is an open-source platform for developing, scheduling, and monitoring batch-oriented workflows. Airflow’s extensible Python framework enables you to build workflows connecting with virtually any technology. A web interface helps manage the state of your workflows. Airflow is deployable in many ways, varying from a single process on your laptop to a distributed setup to support even the biggest workflows.

### Usefull Links -
- https://airflow.apache.org/docs/apache-airflow/stable/index.html
- https://www.devopsschool.com/blog/what-is-apache-airflow-and-use-cases-of-apache-airflow/

### High level approach 

1. Create Docker file
1. Use docker build to run the container from docker file
1. Mount the Dags folder from local system to the docker conatiner
2. Expose the docker container on the listener port to access the Airflow GUI
3. https://harshalpagar.medium.com/running-airflow-with-docker-on-developer-environment-9c1c9559668c

### Rquired Tools

1. Install Docker engine
```
sudo apt install docker.io -y 
```

2. Install python
```
sudo apt install python3.8 -y 
```

3. Install docker compose
```
sudo apt install docker-compose
docker compose version
```
### 1. Create new Dockerfile and copy the below code to it

```
FROM apache/airflow:2.5.1-python3.9

COPY requirements.txt /requirements.txt

RUN pip install --no-cache-dir -r /requirements.txt

USER root

RUN apt-get update && apt-get install -y \
    wget

COPY start.sh /start.sh
RUN chmod +x /start.sh
USER airflow
ENTRYPOINT ["/bin/bash","/start.sh"]
```
### 2. Create start.sh file to run the sets of Airflow command at once 
```
#!/bin/bash
airflow standalone
```
### 3. Build the docker image from the docker file
```
docker build . -t airflow-local
```
- Note : Keep all the required files for docker image build in one folder 

![image](https://github.com/anand40090/Airflow-Docker/assets/32446706/c6c3b9d5-f84f-4b5d-8135-43217db6ec0c)

- Verify the created docker image

![image](https://github.com/anand40090/Airflow-Docker/assets/32446706/49c599c5-b370-4669-9a0d-a318dbe99c29)


### 4. Use the create Apache-Airflow docker image to run the container 
```
docker run -p 8080:8080 -v /home/admin1/apache-airflow/dags:/opt/airflow/dags  -d airflow-local

# Here replace local path of airflow DAG's repository, so that if you made changes to dags in the folder we don't have to restart docker
```
Output - 

![image](https://github.com/anand40090/Airflow-Docker/assets/32446706/2a8a9d6d-f3fe-40d1-8d63-d6b3954c154b)



