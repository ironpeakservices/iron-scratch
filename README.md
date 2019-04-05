# ironpeakservices/go-scratch
Secure base image for running Go applications.
The default entrypoint is `/app`.

Check it out [on Docker Hub](https://hub.docker.com/r/ironpeakservices/go-scratch)!


## How is this different?
This is based on the empty scratch image, but contains two additional things:
- CA Certificates for verifying certificates
- An unprivileged user

## Example
```
FROM golang:alpine AS builder
COPY main.go /
RUN go build -ldflags '-w -s -extldflags "-static"' -o /app /

FROM ironpeakservices/go-scratch
COPY --from=builder /app /app
```
