# ironpeakservices/iron-scratch
Secure base image for running Go applications.
The default entrypoint is `/app`.

`docker pull docker.pkg.github.com/ironpeakservices/iron-scratch/iron-scratch:3.10.3`


## How is this different?
This is based on the empty scratch image, but contains two additional things:
- CA Certificates for verifying certificates ([location info](https://golang.org/src/crypto/x509/root_linux.go))
- An unprivileged user

## Example
```
FROM golang:alpine AS builder
COPY main.go /
RUN go build -ldflags '-w -s -extldflags "-static"' -o /app /main.go

FROM docker.pkg.github.com/ironpeakservices/iron-scratch/iron-scratch:3.10.3
COPY --from=builder /app /app
```

## Update policy
Updates to the official alpine docker image are automatically created as a pull request and trigger linting & a docker build.
When those checks complete without errors, a merge into master will trigger a deploy with the same version to packages.
