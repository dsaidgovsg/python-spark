# python-spark

This image is based off the [`python:2.7`](https://hub.docker.com/_/python/) image and
contains Hadoop, Sqoop and Spark binaries. Installs OpenJDK 7.

This is used as a base image for [`airflow-pipeline`](https://github.com/datagovsg/airflow-pipeline), a simplified setup for Airflow to launch Hadoop and Spark jobs.

Useful packages included for Spark and Sqoop:
- Spark-csv
- Spark-avro
- MySQL JDBC driver
- PostgreSQL JDBC driver
- MS SQL JDBC driver

## Kerberos support
Kerberos support has been added, but derivative images would need to update the kerberos configuration file in `/etc/krb5.conf`. To use spark with kerberos, the
environment variable `PYSPARK_SUBMIT_ARGS` should also be modified to specify the kerberos principal and location of the corresponding keytab. Derivative images
are responsible for putting in the keytab by mounting the volume containing the keytab into the image (suggested location: /etc/kerberos). So for example, if
the principal is `kerbuser` and the keytab is `kerbuser.keytab` in the default location, then `PYSPARK_SUBMIT_ARGS` would be

`[other spark parameters] --principal kerbuser --keytab /etc/kerberos/kerbuser.keytab pyspark-shell`

## Logging in to Docker Hub Container Registry
Credentials are managed by `docker login`. Login with `docker login`

## Building and pushing new images
Assuming that the Python version we want to build is `2.7`, the Hadoop version is `2.7.3` and the Spark version is `2.1.0`, then we can build the images as such:

```bash
docker build -t datagovsg/python-spark:2.7-2.1 -f python2/spark2.1/Dockerfile .
docker push datagovsg/python-spark:2.7-2.1
```

## Supported tags and respective `Dockerfile` links

- `latest`: Python 2.7 with Spark 1.6.1 ([Dockerfile](Dockerfile))
- `2.7-1.6`: Python 2.7 with Spark 1.6.1 ([Dockerfile](Dockerfile))
- `2.7-2.1`: Python 2.7 with Spark 2.1.0 ([Dockerfile](Dockerfile))

## Spark version specific things to take note
- `PYTHONPATH` and `PYSPARK_SUBMIT_ARGS`
- Spark packages installed into the Ivy cache
- Spark 1.6.1 is good up to Cloudera CDH 5.9
- Spark 1.6.2 is good up to Cloudera CDH 5.10
