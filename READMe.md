# Cynthus emulators

Local stand-ins for the cloud services, started with `docker compose up -d` on the `cynthus_net` network.

## Mail sink

`mail` receives email over SMTP and keeps it, so email features can be built and tested without sending real mail.
Nothing is delivered outside the host.

- **Connect from a service:** SMTP host `mail.cynthus.local`, port `1025`, no TLS, any login (or none).
- **Read the inbox:** open `http://<host>:8025/` (change the port with `MAIL_UI_PORT`).
- **Read it from a test or script:** `GET http://<host>:8025/api/v1/messages` lists messages, and
  `GET /api/v1/message/latest` returns the newest, including its text, so a test can find a verification link.
- **Clear it:** `DELETE /api/v1/messages`.
