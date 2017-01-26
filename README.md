# python-spark

This image is based off the [`python:2.7`](https://hub.docker.com/_/python/) image and
contains Hadoop, Sqoop and Spark binaries.

This is used as a base image for [`airflow-pipeline`](https://github.com/datagovsg/airflow-pipeline), a simplified setup for Airflow to launch Hadoop and Spark jobs.

Useful packages included for Spark and Sqoop:
- Spark-csv
- Spark-avro
- MySQL JDBC driver
- PostgreSQL JDBC driver
- MS SQL JDBC driver

## Logging in to Docker Hub Container Registry
Credentials are managed by `docker login`. Login with `docker login`

## Building and pushing new images
Assuming that the Python version we want to build is `2.7`, the Hadoop version is `2.7.3` and the Spark version is `2.0.1`, then we can build the images as such:

```bash
docker build -t datagovsg/python-spark:2.7-2.0 -f Dockerfile-2.0 .
docker push datagovsg/python-spark:2.7-2.0
```

## Supported tags and respective `Dockerfile` links

- `latest`: Python 2.7 with Spark 1.6.1 ([Dockerfile](Dockerfile))
- `2.7-1.6.1`: Python 2.7 with Spark 1.6.1 ([Dockerfile](Dockerfile))
- `2.7-2.0`: Python 2.7 with Spark 2.0.1 ([Dockerfile](Dockerfile))

## Spark version specific things to take note
- `PYTHONPATH` and `PYSPARK_SUBMIT_ARGS`
- Spark packages installed
