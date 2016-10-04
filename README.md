# `python-spark`

This image is based off the [`python:2.7`](https://hub.docker.com/_/python/) image and
contains Hadoop, Sqoop and Spark binaries.

Useful packages included for Spark and Sqoop:
- Spark-csv
- Spark-avro
- MySQL JDBC driver
- PostgreSQL JDBC driver
- MS SQL JDBC driver

## Logging in to Docker Hub Container Registry
Credentials are managed by `docker login`. Login with `docker login hub.docker.com`

## Building and pushing new images
Assuming that the Python version we want to build is `2.7` and the Spark version is `1.6.1`, then we can build
the images as such:

```bash
docker build -t hub.docker.com/datagovsg/python-spark:2.7-1.6.1 .
docker push hub.docker.com/datagovsg/python-spark:2.7-1.6.1
```

## Supported tags and respective `Dockerfile` links

- `2.7-1.6.1`: Python 2.7 with Spark 1.6.1 ([Dockerfile](Dockerfile))
