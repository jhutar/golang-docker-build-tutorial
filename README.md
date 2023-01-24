# Project Template: Create a Docker image for a Go application
[![Docker workflow status](https://github.com/miguno/golang-docker-build-tutorial/actions/workflows/docker-image.yml/badge.svg)](https://github.com/miguno/golang-docker-build-tutorial/actions/workflows/docker-image.yml)
[![Go workflow status](https://github.com/miguno/golang-docker-build-tutorial/actions/workflows/go.yml/badge.svg)](https://github.com/miguno/golang-docker-build-tutorial/actions/workflows/go.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

A template project to create a Docker image for a Go application.
The example application exposes an HTTP endpoint.

> **Java developer?** Check out https://github.com/miguno/java-docker-build-tutorial

Features:

* The Docker build uses a
  [multi-stage build setup](https://docs.docker.com/build/building/multi-stage/)
  to minimize the size of the generated Docker image, which is 5MB
* Supports [Docker BuildKit](https://docs.docker.com/build/)
* Golang 1.19
* [GitHub Actions workflows](https://github.com/miguno/golang-docker-build-tutorial/actions) for
  [Golang](https://github.com/miguno/golang-docker-build-tutorial/actions/workflows/go.yml)
  and
  [Docker](https://github.com/miguno/golang-docker-build-tutorial/actions/workflows/docker-image.yml)

# Requirements

Docker must be installed on your local machine. That's it. You do not need to
have Go installed.

# Usage and Demo

**Step 1:** Create the Docker image according to [Dockerfile](Dockerfile).
This step builds, tests, and packages the [Go application](app.go).
The resulting image is 5MB in size.

```shell
# ***Creating an image may take a few minutes!***
$ docker build --build-arg PROJECT_VERSION=1.0.0-alpha -t miguno/golang-docker-build-tutorial:latest .

# You can also build with the new BuildKit.
# https://docs.docker.com/build/
$ docker buildx build --build-arg PROJECT_VERSION=1.0.0-alpha -t miguno/golang-docker-build-tutorial:latest .
```

Optionally, you can check the size of the generated Docker image:

```shell
$ docker images miguno/golang-docker-build-tutorial
REPOSITORY                            TAG       IMAGE ID       CREATED          SIZE
miguno/golang-docker-build-tutorial   latest    2de05b854c1b   11 minutes ago   4.78MB
```

**Step 2:** Start a container for the Docker image.

```shell
$ docker run -p 8123:8123 miguno/golang-docker-build-tutorial:latest
```

**Step 3:** Open another terminal and access the example API endpoint of the
running container.

```shell
$ curl http://localhost:8123/status
{"status": "idle"}
```

# Notes

You can run the Go application locally if you have Go installed.
See [justfile](justfile) for additional commands and options.

```shell
# Build
$ go build -trimpath -ldflags="-w -s" -v -o app cmd/golang-docker-build-tutorial/main.go

# Test
$ go test -cover -v ./...

# Run
$ ./app
```
