# Dockerize Traefik

This project was heavily influenced by
[this](https://www.smarthomebeginner.com/traefik-reverse-proxy-tutorial-for-docker/)
tutorial and
[this](https://github.com/htpcBeginner/AtoMiC-ToolKit-Docker)
project.

## Prerequisites

*   [Docker](https://docs.docker.com/install/)
*   [Docker Compose](https://docs.docker.com/compose/install/)
*   A linux box to deploy to ;)

## Up and Running

1.  Create a directory that will serve as the home for your docker services.

    ```sh
    mkdir ~/docker && cd ~/docker
    ```

1.  Clone this repo into the newly created directory as "traefik".

    ```sh
    git clone https://github.com/sonofborge/dockerize-traefik.git traefik && cd traefik
    ```

1.  Create `.env` file and modify the variables to fit your needs.

    ```sh
    cp .env-example .env
    vim .env
    ```

    ```sh
    # Example: .env file

    CLOUDFLARE_API_KEY=mycloudflareapikey
    CLOUDFLARE_EMAIL=admin@domain.com
    DOMAINNAME=domain.com
    HTTP_USERNAME=admin
    HTTP_PASSWORD=mysuperstrongpassword
    PUID=1000
    PGID=999
    TZ=America/Los_Angeles
    ```

1.  Create "acme.json".

    ```sh
    touch acme/acme.json && chmod 600 acme/acme.json
    ```

1.  Create the traefik config file and modify with your information.

    ```sh
    cp config/traefik.example config/traefik.toml
    ```

    ```sh
    # Example: config/traefik.toml file
    # Replace these values in "quotes" with your information.

    ...
    [acme]
    email = "admin@email.com"
    ...
    [[acme.domains]]
      main = "mydomain.com"
    [[acme.domains]]
      main = "*.mydomain.com"
    ...
    [docker]
    ...
    domain = "mydomain.com"
    ```

1.  Create the docker network.

    ```sh
    docker network create traefik_proxy
    ```

1.  Spin up the service.

    ```sh
    docker-compose up -d
    ```

    For troubleshooting and logging,
    run the following command:

    ```sh
    docker-compose logs -tf --tail="50" traefik
    ```

If all went well,
you should now have a traefik reverse proxy all set up!
