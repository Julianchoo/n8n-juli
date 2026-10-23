# Rebuild after the eMudhra/emSign incident

The base image now uses Node 22.23.4, within this n8n version's supported Node 22 range. Derived images use this repository owner's rebuilt base, not the older upstream base.

1. A push changing the base Dockerfile or its workflow on `master` starts `Docker Base Image CI` automatically. It can also be run manually with `node_version=22.23.4`. It publishes `ghcr.io/julianchoo/base:22.23.4` using the repository's GitHub token.
2. Rebuild each n8n image with `--pull --no-cache` after the base workflow completes. For example, from the repository root:

```sh
docker build --pull --no-cache -f docker/images/n8n-custom/Dockerfile -t n8n-security-update .
docker run --rm --entrypoint node n8n-security-update --version
```

3. Deploy the rebuilt image to the n8n host. Updating this repository does not replace an already running container.

If using a different registry, pass `--build-arg N8N_BASE_IMAGE=<your-rebuilt-base-image>` to the derived image build.
