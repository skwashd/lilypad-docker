# Lilypad Connect for Docker

Lightweight container for running the lilypad connect server under docker.

This image uses a multistage build which weighs in at around 4MB.

## Usage

### Base Image

You can use this image as a base image that you extend using your own configuration.

Create your own Dockerfile:

```Dockerfile
FROM skwashd/lilypad-connect:latest

COPY ./connect.yml /data/connect.yml

ENTRYPOINT  ["/lilypad-connect"]
```

### With Volume

Extending this image is probably overkill for most use case. Instead you can mount the config file as a volume.

Copy the `example.connect.yml` file to a directory and rename it to `connect.yml`. Change the password.

Run the container like so:

```sh
docker run --name="lilypad-connect" -v /path/to/config/directory:/data skwashd/lilypad-connect:latest

```

### Docker Compse

Similarly to the volume example above you can use this image as part of a docker compose orchestrated deployment. 

Copy the example connect.yml file and edit it to suit you needs. 

Use the following snippet in your compose file:

```yaml
services:
  connect:
    container_name: connect
    image: skwashd/lilypad-connect:latest
    networks:
      public:
        ipv4_address: 192.168.0.2
      private:
        ipv4_address: 192.168.1.2
    restart: always
    ports:
      - "5091:${connect_port}"
    volumes:
      - ./volumes/connect/data:/data
```
