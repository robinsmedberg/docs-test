k8s_yaml("serve.yaml")

docker_build(
    "docs-v3",
    ".",
    dockerfile="Dockerfile.local",
    live_update=[
        # when package.json changes, we need to do a full build
        fall_back_on(["mkdocs.yml", "docs"]),
        # Map the local source code into the container under /src
        sync("docs", "/docs"),
    ],
)

k8s_resource("docs-v3", port_forwards="8080:8000")
