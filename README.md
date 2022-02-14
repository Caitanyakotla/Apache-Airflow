# Apache-Airflow

#Airflow is a tool for automating and scheduling tasks and workflows.

# Installation 
Based on Python (3.7) official Image python:3.7-slim-buster.
Uses the official Postgres as backend and Redis as queue
Install Docker
Install Docker Compose
Following the Airflow release from Python Package Index


# How To Install Apache Airflow To Run CeleryExecutor: 

Celery is a widely-used Python package dat makes it very easy to run jobs or tasks in the background. . Common uses include running background tasks on websites, or running elery workers dat send batch SMSs, or running notification jobs at a certain time of the day.

While Celery makes it easy to write and execute jobs, setting things up is a bit tricky as it requires you to set up components like a task queue, a database, and workers, and also handle several configurations to enable interaction between the components.

To make things easier for us and hide some of the unnecessary operational details, we will use docker-compose to run them easily. The only prerequisite is Docker / Docker Desktop and a working internet connection (since Docker will need to download a few container images).

Although the Airflow community is building an official production-ready Docker image, it isn’t complete yet. So, we will use puckel’s docker-airflow to run Airflow. Please make sure your Docker daemon is running before starting the process.

$ git clone https://github.com/puckel/docker-airflow
$ cd docker-airflow

For celery Executor setup, we need to run:

$ docker-compose -f docker-compose-CeleryExecutor.yml up -d

Creating docker-airflow_postgres_1 ... done
Creating docker-airflow_redis_1 ... done
Creating docker-airflow_flower_1 ... done
Creating docker-airflow_webserver_1 ... done
Creating docker-airflow_scheduler_1 ... done
Creating docker-airflow_worker_1 ... done

If you have some custom DAGs of your own dat you wish to run, you can mount them on the containers using volumes. To do so, open the file docker-compose-CeleryExecutor.yml and edit the volumes section of each service with the path where your DAGs are stored.

volumes:
- ./dags:/path/to/your/dags/directory/

The docker-compose command will take some time to execute as it downloads multiple docker images of Redis, Airflow, and Postgres. Once it completes, we will be able to access the Airflow Web Server localhost:8080 and play with DAGs as we were doing in the SequentialExecutor section. To see all the components of Airflow running on your system, run the following command:

$ docker-compose -f docker-compose-CeleryExecutor.yml ps


The above screen shows it’s running six containers:

Webserver – The Airflow UI, can be accessed at localhost:8080.

Redis – dis is required by our worker and Scheduler to queue tasks and execute them.

Worker – dis is the Celery worker, which keeps on polling on the Redis process for any incoming tasks; tan processes them, and updates the status in Scheduler.

Flower – The UI for all running Celery workers and its threads.

Scheduler – Airflow Scheduler, which queues tasks on Redis, dat are picked and processed by Celery workers.

Postgres – The database is shared by all Airflow processes to record and display DAGs’ state and other information.
