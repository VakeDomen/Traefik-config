# Traefik v2 setup
This is a Traefik configuration for personal dev server.
Traefik is a open source reverse proxy / load balancer which is raising in popularity because of its ease to setup, integration with Docker and Let’s encrypt and much more features.

[Full Traefik documentation](https://docs.traefik.io/v2.0/)

## Setup
To setup the proxy server, clone the repository to `/etc/traefik/` in your file system. Then create a `acme.json` file to store the certificate data to `/etc/traefik/data/` and change the permissions
```
sudo touch /etc/traefik/data/acme.json
sudo chmod 600 /etc/traefik/data/acme.json
```
Then in `/etc/traefik/` rename the `.env.sample` to `.env` and fill the required enviorment variables for traefik.
The variable `TRAEFIK_DOMAIN` specifies the domain where the traefik dashboard will be served. 
The variable `TRAEFIK_CERTIFICATESRESOLVERS_le_ACME_EMAIL` will require your email, so the let's encrypt certificate resolver can generate certificates.

Last you will need to specify the credentals for logging in to the traefik dashboard by filling the `CREDENTIALS` variable.
First we are going to generate a user/password combination for basic authentication using htpasswd . If you don’t have it installed, you need to do it first (example for Ubuntu server):
```
sudo apt install apache2-utils
```
We can generate the creentials using the command below.
```
echo $(htpasswd -nbB <USER> "<PASS>")
```

The output will be for example (it will be a different result for each time you run the command above):
```
<USER>:$apr1$ryHGa8yK$5lRELezhgkUtJxiJ.XTfZ.
```
## Running Traefik
We can now run the container using the command below.
```
docker-compose up -d
```
Note: You may also need to create an external network named `proxy`.

## Use
The certificate resolver is generated under the name `le` and the entrypoints under `web` for http and `websecure` for https.
To bind a docker-container to the traefik reverse proxy we are now able to use the `label` keyword in our docker-containers.

Also for Traefik to be able to see the container, it should be bound to the `proxy` external network.

You may check my basic working example [here](https://github.com/VakeDomen/Traefik-Whoami).
