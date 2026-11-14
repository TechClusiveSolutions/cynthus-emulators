# cynthus-emulators

## Description

Local stand-ins for cloud services, run with Docker Compose on the `cynthus_net` network: a mail sink that keeps
email instead of sending it, a RabbitMQ broker for the signal bus, and the Azure Service Bus emulator with its SQL
Server store. They stand in for the real services only as far as the Cynthus stacks need.

## Prerequisites

- Docker with the Compose plugin.
- The Docker network `cynthus_net`: `docker network create cynthus_net`.

## Installation

```sh
git clone https://github.com/TechClusiveSolutions/cynthus-emulators.git
cd cynthus-emulators
docker network create cynthus_net
# create .env (see Minimal configuration)
docker compose up -d
docker compose --profile rabbitmq up -d rabbitmq
docker compose ps
```

The second compose command starts RabbitMQ, which is off by default. `docker compose ps` shows the containers as
`healthy` when they are ready.

## Minimal configuration

Create `.env` next to `docker-compose.yml`. It is git-ignored; never commit it.

- `STORE_PASSWORD`: the password of the SQL Server store behind the Service Bus emulator.
- `TIME_ZONE`: for example `UTC`.
- `RABBITMQ_PASSWORD`: required before the first start of RabbitMQ (and `RABBITMQ_USER`, default `cynthus`).
- `MAIL_UI_PORT`: optional, the host port of the mail inbox page (default `8025`).

## Gotchas

- RabbitMQ keeps its user in the `rabbitmq-data` volume, so set `RABBITMQ_PASSWORD` before the first start; changing
  it later does not change the stored user.
- The mail sink delivers nothing outside the host. Read the mail at `http://<host>:8025/`.
- The Service Bus emulator has a small connection limit (10 by default) that the whole Cynthus stack can exceed;
  RabbitMQ has no such limit.
- Services reach the emulators by their network names (`mail.cynthus.local`, `rabbitmq.cynthus.local`), so they must
  be on `cynthus_net`.

## More documentation

- [`docs/mail-sink.md`](docs/mail-sink.md): connecting to the mail sink, reading and clearing the inbox.
- [`docs/rabbitmq.md`](docs/rabbitmq.md): starting the broker, its settings, the dead-letter queues and the data volume.
- [`CHANGELOG.md`](CHANGELOG.md): what changed in each release.
- [Engineering standards](https://github.com/TechClusiveSolutions/cynthus/blob/dev/docs/engineering-standards.md):
  branches, tests, lint and review.
