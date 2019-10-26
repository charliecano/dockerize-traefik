# Dockerize Traefik

This project was heavily influenced by
[this](https://www.smarthomebeginner.com/traefik-reverse-proxy-tutorial-for-docker/)
tutorial and
[this](https://github.com/htpcBeginner/AtoMiC-ToolKit-Docker)
project.

**Note:**
Since the time that the above mentioned resources were authored,
Traefik has undergone a _huge_ change from `1.7` to `2.0`.
Virtually everything about Traefik configuration changed in this major version update.
Be sure to read Traefik's
[documentation](https://docs.traefik.io/)
and look closely at the `docker-compose.yml` file in this project.

## Prerequisites

*   [Docker](https://docs.docker.com/install/)
*   [Docker Compose](https://docs.docker.com/compose/install/)
*   A domain provided by [Cloudflare](https://www.cloudflare.com/)
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

    CF_API_KEY=mycloudflareapikey
    CF_API_EMAIL=admin@domain.com
    DOMAIN_NAME=domain.com
    ```

1.  Generate a user/password hash with `htpasswd` and store the output in `config/usersfile`.
    Be sure to replace "user" and "password" with your own username and password.

    ```sh
    htpasswd -nb user password >> config/usersfile
    ```

1.  Create "acme.json" and restrict file permissions.

    ```sh
    touch letsencrypt/acme.json && chmod 600 letsencrypt/acme.json
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
    tail -f log/accessLog.txt
    tail -v log/traefik.log
    ```

If all went well,
you should now have a traefik reverse proxy all set up!
