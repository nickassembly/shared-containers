# containers/Tiltfile
# Orchestrates shared infrastructure: base-dev image, Ollama, future shared services.

# ─────────────────────────────────────────────────────────────
# Build shared images
# ─────────────────────────────────────────────────────────────

# Build the base-dev image and tag it for the local registry.
# Projects' Dockerfiles will use `FROM localhost:5000/base-dev:latest`.
docker_build(
    'localhost:5000/base-dev',
    context='./base-dev',
    dockerfile='./base-dev/Dockerfile',
)

# ─────────────────────────────────────────────────────────────
# Apply shared infrastructure manifests
# ─────────────────────────────────────────────────────────────

k8s_yaml([
    'k8s/namespace.yaml',
    'k8s/ollama.yaml',
])

# ─────────────────────────────────────────────────────────────
# Resource configuration
# ─────────────────────────────────────────────────────────────

# Ollama: forward port 11434 to the host so you can curl it directly during dev.
# After `tilt up`, `curl http://localhost:11434/api/tags` should list your models.
k8s_resource(
    'ollama',
    port_forwards='11434:11434',
    labels=['shared-infra', 'ai'],
)

# ─────────────────────────────────────────────────────────────
# Tilt UI configuration
# ─────────────────────────────────────────────────────────────

# Disable analytics (offline-friendly, no telemetry)
analytics_settings(False)

# Allow this Tiltfile to manage resources only in the 'shared' namespace
allow_k8s_contexts('k3d-dev')