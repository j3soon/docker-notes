# General Discussions

This page contains paraphrased discussions that may be useful for future reference. Also note that these discussions are searchable from the search bar in the website.

For issues related to this page, please [open a GitHub issue](https://github.com/j3soon/docker-notes/issues).

## `apt-get update` failure in Docker container

Q: How to fix `apt-get update` failure in Docker container due to potential DNS or network issues?

A: Edit Docker configuration (`/etc/docker/daemon.json`):

```json
{
  "dns": ["8.8.8.8", "8.8.4.4"]
}
```

and then restart Docker with `sudo service docker restart`.

Reference:

- [Docker: Temporary failure resolving 'deb.debian.org'](https://stackoverflow.com/a/68199803)

> 2025-06-16.

## Change Docker root directory

TL;DR: Add `data-root` to docker configuration file `/etc/docker/daemon.json`, see more details below.

Reference:

- [Relocating the Docker root directory](https://www.ibm.com/docs/en/z-logdata-analytics/5.1.0?topic=software-relocating-docker-root-directory)

> 2025-12-13.

## Cross-compilation for ARM64

Cross-compilation and multi-architecture images, details below.

References:

- [Multi-platform builds](https://docs.docker.com/build/building/multi-platform/)
- [Running and Building ARM Docker Containers on x86](https://www.stereolabs.com/docs/docker/building-arm-container-on-x86)
