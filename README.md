# OpenTracing demo

This repo contains a demo setup of services using OpenTracing.

## Setup environment

Build Esaco Docker image with tracing:

```console
$ git clone --branch feat/tracing https://github.com/marcocaberletti/esaco
$ cd esaco
$ mvn clean package
$ cd esaco-app/docker
$ sh build-image.sh
```

Start service:

```console
$ docker-compose up --build -d
```

Since ElasticSearch has a startup delay, restart the collector:

```console
$ docker-compose restart jaeger-collector jaeger-ui
```

## Usage

Get an access token from IAM test:

```console
$ export CLIENT_ID=my-client-id
$ export CLIENT_SECRET=my-client-secret
$ export USERNAME=my-username
$ export PASSWORD=my-super-secret-password

$ curl -XPOST -u $CLIENT_ID:$CLIENT_SECRET -d grant_type=password \
    -d username=$USERNAME -d password=$PASSWORD \
    -d scope="openid profile" https://iam-test.indigo-datacloud.eu/token | jq

$ export TOKEN=eyJraWQiOiJyc2ExIi...
```

Query Esaco:

```console
$ curl -u user:password -d token=$TOKEN http://localhost/tokeninfo | jq
```

Navigate Jaeger data pointing the browser to Jaeger UI: http://localhost:16686

Navigate ElasticSearch data point the browser to Kibana: http://localhost:5601