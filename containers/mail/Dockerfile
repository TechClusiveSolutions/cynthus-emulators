FROM axllent/mailpit:v1.21.8

# Accepts SMTP on 1025 and shows the messages on 8025 (web page and API).
# Nothing is relayed: a message that arrives here stays here.
ENV MP_SMTP_AUTH_ACCEPT_ANY=1 \
    MP_SMTP_AUTH_ALLOW_INSECURE=1 \
    MP_MAX_MESSAGES=500
