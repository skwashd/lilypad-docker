# Lilypad Proxy for Docker

Lightweight container for running the goalng lilypad proxy server under docker.

Once build this image is less than 8MB in size.

## Usage

### Base Image

You can use this image as a base image that you extend using your own configuration.

Create your own Dockerfile:

```Dockerfile
FROM skwashd/lilypad-proxy:latest

COPY ./connect.yml /data/proxy.yml

ENTRYPOINT  ["/lilypad-proxy"]
```

### With Volume

Extending this image is probably overkill for most use case. Instead you can mount the config file as a volume.

Copy the `example.connect.yml` file to a directory and rename it to `connect.yml`. Change the password.

Run the container like so:

```sh
docker run --name="lilypad-proxy" -v /path/to/config/directory:/data skwashd/lilypad-proxy:latest

```

### Docker Compse

Similarly to the volume example above you can use this image as part of a docker compose orchestrated deployment. 

Copy the example connect.yml file and edit it to suit you needs. 

Use the following snippet in your compose file:

```yaml
services:
  proxy:
    container_name: proxy
    image: pfizer/hemocraft-proxy:latest
    networks:
      public:
        ipv4_address: 192.168.0.3
      private:
        ipv4_address: 192.168.1.3
    restart: always
    ports:
      - "25565:${proxy_port}"
    volumes:
      - ./volumes/proxy/data:/data
```
