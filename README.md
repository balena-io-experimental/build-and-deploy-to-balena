# build-and-deploy-to-balena
Workflow for using buildx to build and deploy a release to a balena environment while supporting one or more architectures.

## Jobs

1. `prepare`: Generates metadata and build matrix
2. `build-and-push`: Runs in parallel on arm64 and amd64 runners, pushing to separate tags:
    - ARM: ghcr.io/repo/service:main-a36e43a-arm
    - AMD: ghcr.io/repo/service:main-a36e43a-amd
3. `create-manifest`: Waits for both builds, then combines them into a multi-arch manifest: `ghcr.io/$OWNER/$REPO/$SERVICE:$BRANCH$-$REF (contains all platforms)
4. `deploy-to-balena`: Pulls the multi-arch manifest and deploys to balena