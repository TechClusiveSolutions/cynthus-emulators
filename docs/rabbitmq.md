# RabbitMQ broker

`rabbitmq` is the broker for `SIGNAL_BROKER=rabbitmq` (ADR-007 in the `cynthus` documentation repository). It has no
connection limit of the Service Bus emulator's kind, so the whole stack can run against it. It is off by default.

- **Start it:** `docker compose --profile rabbitmq up -d rabbitmq`. Without the profile, `docker compose up -d` does not
  start it.
- **Settings in `.env`:** `RABBITMQ_USER` (default `cynthus`), `RABBITMQ_PASSWORD` (required: set it before the first
  start; the broker keeps the user in its data volume) and `RABBITMQ_UI_PORT` (default `15672`). `.env` is never
  committed.
- **Connect from a service:** set `SIGNAL_BROKER=rabbitmq` and
  `RABBITMQ_URL=amqp://<user>:<password>@rabbitmq.cynthus.local:5672/` in the service's untracked `.env`.
- **Look at it:** open `http://<host>:15672/` and sign in with the user and password above. A signal that its handler
  rejected waits in the queue `<subscription>.dlq`.
- **No queue definitions here:** each service declares its own exchange, queue and dead-letter queue when it starts
  (`docs/cynthus/messaging/broker.md` in cynthus-shared).
- **Data:** kept in the `rabbitmq-data` volume. `docker compose --profile rabbitmq down -v` removes it. If you change a
  queue's delivery limit or TTL, delete the old queue first; RabbitMQ refuses to redeclare it with other settings.
- **Why it runs as the `rabbitmq` user:** started as root, the image drops privileges with `gosu` and then cannot read
  its own Erlang cookie under Colima (`eacces`).
