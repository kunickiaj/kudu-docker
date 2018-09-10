# Docker image for Apache Kudu
![Apache Kudu](https://d3dr9sfxru4sde.cloudfront.net/i/k/apachekudu_logo_0716_345px.png)

## What is Kudu?
Kudu is an open source storage engine for structured data which supports low-latency random access together with efficient analytical access patterns.  Kudu distributes data using horizontal partitioning and replicates each partition using Raft consensus, providing low mean-time-to-recovery and low tail latencies. Kudu is designed within the context of the Hadoop ecosystem and supports many modes of access via tools such as [Apache Impala (incubating)](https://impala.apache.org/), [Apache Spark](https://spark.apache.org/), and [MapReduce](https://hadoop.apache.org/).

[https://kudu.apache.org/](https://kudu.apache.org/)

## How to use this image?

## The Easy Way

### Using docker-compose
```bash
docker-compose up -d
```

### Starting a Kudu console
```bash
docker run --rm -it --link kududocker_kudu-tserver_1:kudu_tserver -e KUDU_TSERVER=kudu_tserver kunickiaj/kudu cli status
```

## The Long Way

### Pull this Docker image
```bash
docker pull kunickiaj/kudu
```

### (Optional) Building this Docker image
```bash
docker build -t kunickiaj/kudu .
```

### (Optional) Create Data containers
```bash
docker create --name kudu-master-data -v /var/lib/kudu/master kunickiaj/kudu
docker create --name kudu-tserver-data -v /var/lib/kudu/tserver kunickiaj/kudu
```

### Starting the Kudu Master
```bash
docker run -d --name kudu-master -p 8051:8051 kunickiaj/kudu master
```

### Starting the Kudu TabletServer
```bash
docker run -d --name kudu-tserver -p 8050:8050 --link kudu-master \
  -e KUDU_MASTER=kudu-master kunickiaj/kudu tserver
```

### Tailing the logs
```bash
docker logs -f kudu-master
docker logs -f kudu-tserver
```

### Starting a Kudu console
```bash
docker run --rm -it --link kudu-tserver -e KUDU_TSERVER=kudu-tserver kunickiaj/kudu kudu tserver status kudu-tserver
```

### Accessing the web interfaces
Each component provide its own web UI. Open you browser at one of the URLs below, where `dockerhost` is the name / IP of the host running the docker daemon. If using Linux, this is the IP of your linux box. If using OS X or Windows (via Docker-Machine), you can find out your docker host by typing `docker-machine ip default`.

| Component               | Port                                              |
| ----------------------- |-------------------------------------------------- |
| Master                  | [http://dockerhost:8051](http://dockerhost:8051)  |
| TabletServer            | [http://dockerhost:8050](http://dockerhost:8050)  |


### Other links
- This docker image (and README) inspired by https://github.com/bigdatafoundation/docker-kudu
- https://github.com/cloudera/kudu-examples/wiki/Docker-based-tutorial
