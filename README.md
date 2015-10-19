# MongoDB Dockerfile

A Dockerfile that produces a Docker Image for [MongoDB](http://www.mongodb.org/).

## MongoDB version

The `master` branch currently hosts MongoDB 3.0.

Different versions of MongoDB are located at the github repo [branches](https://github.com/frodenas/docker-mongodb/branches).

## Usage

### Build the image

To create the image `frodenas/mongodb`, execute the following command on the `docker-mongodb` folder:

```
$ docker build -t frodenas/mongodb .
```

### Run the image

To run the image and bind to host port 27017:

```
$ docker run -d --name mongodb -p 27017:27017 frodenas/mongodb
```

If you want also to expose the MongoDB http interface, you will need also to expose port 28017:

```
$ docker run -d --name mongodb -p 27017:27017 -p 28017:28017 frodenas/mongodb --httpinterface
```

The first time you run your container,  a new user `mongo` with all privileges will be created with a random password.
To get the password, check the logs of the container by running:

```
docker logs <CONTAINER_ID>
```

You will see an output like the following:

```
========================================================================
MongoDB User: "mongo"
MongoDB Password: "ZMUgiS3O1kJH1ec5"
MongoDB Database: "admin"
MongoDB Role: "dbAdminAnyDatabase"
========================================================================
```

#### Credentials

If you want to preset credentials instead of a random generated ones, you can set the following environment variables:

* `MONGODB_USERNAME` to set a specific username
* `MONGODB_PASSWORD` to set a specific password

On this example we will preset our custom username and password:

```
$ docker run -d \
    --name mongodb \
    -p 27017:27017 \
    -e MONGODB_USERNAME=myusername \
    -e MONGODB_PASSWORD=mypassword \
    frodenas/mongodb
```

#### Databases

If you want to create a database at container's boot time, you can set the following environment variables:

* `MONGODB_DBNAME` to create a database
* `MONGODB_ROLE` to grant the user a role to the database (by default `dbOwner`)

On this example we will preset our custom username and password and we will create a database with the default role:

```
$ docker run -d \
    --name mongodb \
    -p 27017:27017 \
    -e MONGODB_USERNAME=myusername \
    -e MONGODB_PASSWORD=mypassword \
    -e MONGODB_DBNAME=mydb \
    frodenas/mongodb
```

#### Extra arguments

When you run the container, it will start the MongoDB server without any arguments. If you want to pass any arguments,
just add them to the `run` command:

```
$ docker run -d --name mongodb -p 27017:27017 frodenas/mongodb --smallfiles
```

#### Persistent data

The MongoDB server is configured to store data in the `/data` directory inside the container. You can map the
container's `/data` volume to a volume on the host so the data becomes independent of the running container:

```
$ mkdir -p /tmp/mongodb
$ docker run -d \
    --name mongodb \
    -p 27017:27017 \
    -v /tmp/mongodb:/data \
    frodenas/mongodb
```

## Copyright

Copyright (c) 2014 Ferran Rodenas. See [LICENSE](https://github.com/frodenas/docker-mongodb/blob/master/LICENSE) for details.
