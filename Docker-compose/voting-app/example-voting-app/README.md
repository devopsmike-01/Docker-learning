# Example Voting App

A simple distributed application running across multiple Docker containers.

This solution uses `Python`, `Node.js`, `.NET`, with `Redis` for messaging and `Postgres` for storage.

## Architecture

![Architecture diagram](architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time

## Configuration steps

1. To deploy the application with `docker-compose` using pre-built images, 

- Move to the Docker-compose directory

```
cd Docker-compose
```

- To run the application, run in this directory:

```
docker compose up -d
```

2. To deploy the application with `docker-compose` using source files and directories i.e. `healthchecks`, `result`, `seed-data`, `vote`, `worker` each container their respective dockerfiles,

- While in the `Docker-compose` directory, move to the `voting-app` directory and run the following command in this directory to build and run the app:

```
docker compose up -d
```

The `vote` app will be running at [http://ince:5000](http://localhost:5000), and the `results` will be at [http://localhost:5001](http://localhost:5001).

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to deal with them in Docker at a basic level.
