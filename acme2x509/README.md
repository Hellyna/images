# hellyna/acme2x509

This is a tool to convert acme.json files generated by traefik to x509 pem certificates.

# Usage

```bash
docker run --rm \
  -v /path-to/acme.json:/acme.json:ro \
  -v /path-to/certs:/certs
  hellyna/acme2x509 /acme.json /certs
```
