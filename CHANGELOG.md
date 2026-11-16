# Changelog

All notable changes to this deployment definition are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- README rewritten with upstream links, configuration reference and project icon.

## [0.0.1]

### Added

- Initial App-as-Code service definition for ONLYOFFICE Docs (image `onlyoffice/documentserver:10.0`).
- Web interface on port 80 (published as 12888) with Traefik routing and automatic DNS.
- JWT request validation with configurable header, plus persistent volumes for logs, data, cache and the internal PostgreSQL database.
- README with configuration reference and project icon.
