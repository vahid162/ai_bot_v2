# Project-local Specify CLI

This tooling runs Specify CLI only in an ephemeral Docker container for this
repository. It does not install Python, uv, pipx, or Specify CLI on WebMaster.

## Usage

```sh
./specify version
./specify --help
```

The launcher uses only the already-built local image. It does not build, pull,
or contact a registry during normal use.

The image and CLI are pinned:

- uv image: 0.12.5 with Python 3.12, pinned by the linux/amd64 digest
- Specify CLI: 0.16.4
- all resolved Python dependencies: pinned in `constraints.txt`

## Isolation

- The container is removed after every command.
- It has no network, ports, Docker socket, Linux capabilities, or privileged mode.
- Its root filesystem is read-only.
- Only this repository is mounted read-write at `/workspace`.
- The pre-existing `backup` directory is hidden by an inaccessible tmpfs mount.
- Runtime CPU, memory, and process counts are bounded.

The image build needs network access once to retrieve the pinned base image and
the pinned Specify CLI package. Runtime commands that require internet access,
such as update checks or catalog downloads, are intentionally unavailable.

If the local image is intentionally removed, rebuild it from the repository root:

```sh
sudo docker compose -p guardbot-specify -f compose.specify.yaml build --pull specify
```

Do not run `specify self upgrade` in this container. Upgrade by changing both
the pinned Dockerfile version and the project scaffold version in a reviewed
tooling-only change.
