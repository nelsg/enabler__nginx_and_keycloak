# Enabler with Keycloak+NGinx

This enabler shows how to configure NGinx to use a backend Keycloak.

## Installation

### Prerequisites

You need to install **Docker** and **docker-compose**.

### Generate certificates

Execute this commands once from current folder :

```bash
# Generate root autorigned certificate
openssl req -x509 -nodes -new -sha256 -days 1024 -newkey rsa:2048 -subj "/C=FR/CN=NGinx-Root-CA" \
    -keyout ./certs/RootCA.key \
    -out ./certs/RootCA.pem
openssl x509 -outform pem \
    -in ./certs/RootCA.pem \
    -out ./certs/RootCA.crt

# Server certificate
openssl req -new -nodes -newkey rsa:2048 -subj "/C=FR/ST=IdF/L=Paris/O=NELSGEnabler/CN=localhost.local" \
    -keyout ./certs/localhost.key \
    -out ./certs/localhost.csr
openssl x509 -req -sha256 -days 1024 -CA ./certs/RootCA.pem -CAkey ./certs/RootCA.key -CAcreateserial -extfile ./certs/domains.ext \
    -in ./certs/localhost.csr \
    -out ./certs/localhost.crt
```

### Launch NGinx and Keycloak

Then launch components with this command : `docker-compose up`

Home page can be reached here : <https://localhost>. Keycloack button link works when Keycloak was fully started (around 10-20s)

To stop docker, use keys *Ctrl+C*

## References

* [Generate autosigned certificates](https://gist.github.com/cecilemuller/9492b848eb8fe46d462abeb26656c4f8)
* [Configure NGinx and Keycloak](https://itnext.io/nginx-as-reverse-proxy-in-front-of-keycloak-21e4b3f8ec53)
