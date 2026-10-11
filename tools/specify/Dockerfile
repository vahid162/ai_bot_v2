FROM ghcr.io/astral-sh/uv:0.12.5-python3.12-trixie-slim@sha256:0d05436f6b7b8c88236dcaeab65c2b819df944e9af0be7f4b3a2117c38fe868f

ARG SPECIFY_CLI_VERSION=0.16.4

LABEL org.opencontainers.image.title="guardbot-specify-cli" \
      org.opencontainers.image.description="Project-local Specify CLI tooling for guardbot" \
      org.opencontainers.image.version="${SPECIFY_CLI_VERSION}" \
      org.opencontainers.image.source="https://github.com/github/spec-kit"

ENV UV_TOOL_BIN_DIR=/opt/specify/bin \
    UV_TOOL_DIR=/opt/specify/tools \
    UV_CACHE_DIR=/tmp/uv-cache \
    UV_NO_CACHE=1 \
    UV_NO_CONFIG=1 \
    UV_NO_SOURCES=1 \
    UV_PYTHON_DOWNLOADS=never \
    UV_INDEX_STRATEGY=first-index \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PATH="/opt/specify/bin:${PATH}"

COPY constraints.txt /opt/specify/constraints.txt

RUN uv tool install \
    --constraints /opt/specify/constraints.txt \
    --no-build \
    "specify-cli==${SPECIFY_CLI_VERSION}"

USER 1000:1000
WORKDIR /workspace

ENTRYPOINT ["specify"]
CMD ["version"]
