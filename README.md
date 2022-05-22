##### 09.Docker
[Docker image](https://github.com/users/artem-devitsky/packages/container/package/docker_build)
[Github Repo](https://github.com/artem-devitsky/09.Docker)

##docker-compose.yml
```yaml
version: "3.3"
services:
        web:
                build:
                        context: .
                ports:
                      - "80:80"
```

##Dockerfile
```bash
FROM python:alpine
MAINTAINER Artsiom Dziavitski 
COPY test_app.py /test_app.py
ENTRYPOINT ["python3","-u", "test_app.py"]
LABEL org.opencontainers.image.source https://github.com/artem-devitsky/09.Docker
```

##GHCR Workflow
```yaml
name: Docker image publishing

on:
  release:
    types: [published]
  push:
    branches:
      - "master"
jobs:
  push_to_registries:
    name: Push Docker image
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
    steps:
      - name: Check out the repo
        uses: actions/checkout@v2

      - name: Log in to the Container registry
        uses: docker/login-action@v1
        with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.TOKEN }}

      - name: Build and push Docker images
        uses: docker/build-push-action@v2
        with:
         context: .
         push: true
         tags: ghcr.io/artem-devitsky/docker_build:latest

```


