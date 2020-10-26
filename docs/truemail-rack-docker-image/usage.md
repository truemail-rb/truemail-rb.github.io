# Usage

## Requirements

!> docker, docker-compose

## Configuring

For configuring your Truemail Server container please use [available Truemail server options](https://truemail-rb.org/truemail-rack/#/starting-the-server?id=configurable-options ':target=_self'), represented as environment variables.

## Pull and run

Example of usage with docker-compose (Truemail rack image hosted on [dockerhub](https://hub.docker.com/r/truemail/truemail-rack)):

```yml
# docker-compose.yml

version: "3.7"

services:
  truemail:
    image: truemail/truemail-rack:latest
    ports:
      - 9292:9292
    environment:
      VERIFIER_EMAIL: your_email@example.com
      ACCESS_TOKENS: your_token
    tty: true
```

?> If you want to use specific version you should specify it in image section: `image: truemail/truemail-rack:v0.2.8` List of all available releases you can find [here](https://github.com/truemail-rb/truemail-rack-docker-image/releases).
