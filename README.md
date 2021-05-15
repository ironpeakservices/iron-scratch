# ironpeakservices/iron-scratch

Secure base image for running statically compiled applications.
The default entrypoint is `/app`.

`docker pull ghcr.io/ironpeakservices/iron-scratch/iron-scratch:1.0.0`


## How is this different?

This is based on the empty scratch image, but contains additional things:
- CA Certificates for verifying certificates ([location info](https://golang.org/src/crypto/x509/root_linux.go))
- Timezone files
- An unprivileged user

## Example

```dockerfile
FROM golang:alpine AS builder
ENV GOOS=linux GOARCH=amd64 CGO_ENABLED=0
RUN go build std
COPY main.go /
RUN go build -trimpath -ldflags '-w -s -extldflags "-static"' -o /app /main.go

FROM ghcr.io/ironpeakservices/iron-scratch/iron-scratch:1.0.0
COPY --from=builder /app /app
ENTRYPOINT ["/app"]
```

## Update policy

Updates to the official alpine docker image are automatically created as a pull request and trigger linting & a docker build.
When those checks complete without errors, a merge into master will trigger a deploy with the same version to packages.
