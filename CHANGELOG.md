# Changelog

All notable changes to cynthus-emulators are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the versions follow
[Semantic Versioning](https://semver.org/). The Cynthus ecosystem release that bundles
each version is listed in the `cynthus` documentation repository.

Every pull request adds a line under `Unreleased`; a release renames that section to the
version and date.

## [Unreleased]

- Unix line endings (issue 32): `.gitattributes` (`* text=auto eol=lf`), `.editorconfig`, every text file committed with LF, and a CI check (`.github/scripts/check_line_endings.py`) that rejects CRLF.
- README follows the standard quick-start structure (issue 30; cynthus#57): Description, Prerequisites, Installation, Minimal configuration, Gotchas, More documentation. The longer sections moved unchanged into `docs/`. `check_markdown.py` now checks the README headings.
- Documentation for screen readers (issue 28): Markdown is ASCII only, has no tables (lists instead), names the language of every code block, and `README.md` has the standard name. `.github/scripts/check_markdown.py` now enforces this, and checks that relative links resolve, in the `docs-lint` job; markdownlint covers every Markdown file.
- Added a RabbitMQ 4.4 broker (with the management page) to the dev stack for services that run with `SIGNAL_BROKER=rabbitmq`. It sits behind the `rabbitmq` compose profile, so a plain `docker compose up -d` is unchanged. It joins `cynthus_net` as `rabbitmq.cynthus.local`, takes its user and password from `RABBITMQ_USER` and `RABBITMQ_PASSWORD` in the untracked `.env`, keeps its data in a volume and has a health check. It runs as the `rabbitmq` user because the image's root-then-gosu start fails under Colima. The services declare their own queues, so there is no queue configuration here. Issue 26.

## [0.1.0] - 2026-11-08

- Add the Logistics service subscription cynthus-sub-core-logistics (#22) (#23)
- Add the Chat service subscription cynthus-chat-public-sub (#20) (#21)
- Rename the Scheduler subscription to cynthus-sub-scheduler (#18) (#19)
- Add the Shopping service subscription to the emulator (#16) (#17)
- Add the cynthus-sub-core-scheduler subscription (#13) (#14)
- Add a local mail sink (SMTP) for email features (#11)
- Add the Recipe service subscription to the emulator (#9) (#10)
- Add the Worker's signal subscription to the emulator (#8)
- Add the Profile service subscription to the emulator (#5)
- Adopt the Cynthus CI, lint and pull-request workflow (#1) (#3)
- Add Service Bus emulator sample configuration
- Rename config file to Config.json for messaging service
- Add CONFIG_PATH to environment for messaging service
- Add service-bus config for the messaging service
- Initial commit
