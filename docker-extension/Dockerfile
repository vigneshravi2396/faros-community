FROM --platform=$BUILDPLATFORM node:17.7-alpine3.14 AS client-builder
ARG TARGETARCH
WORKDIR /ui
# cache packages in layer
COPY ui/package.json /ui/package.json
COPY ui/package-lock.json /ui/package-lock.json
RUN --mount=type=cache,target=/usr/src/app/.npm \
    npm set cache /usr/src/app/.npm && \
    npm ci
# install
COPY ui /ui
RUN npm run build
COPY docker-compose_${TARGETARCH}.yaml /compose/docker-compose.yaml

FROM alpine
LABEL org.opencontainers.image.title="Faros" \
    org.opencontainers.image.description="Faros extension for Docker Desktop" \
    org.opencontainers.image.vendor="Faros AI, Inc." \
    com.docker.desktop.extension.api.version="0.3.3" \
    com.docker.extension.screenshots="[{\"alt\":\"Faros Architecture\",\"url\":\"https://raw.githubusercontent.com/faros-ai/faros-community-edition/main/img/architecture.png\"}]" \
    com.docker.extension.detailed-description="" \
    com.docker.extension.publisher-url="https://github.com/faros-ai/faros-community-edition" \
    com.docker.extension.additional-urls="" \
    com.docker.extension.changelog="" \
    com.docker.desktop.extension.icon="https://raw.githubusercontent.com/faros-ai/faros-community-edition/main/img/faros.ico" \
    com.docker.extension.categories="utility-tools"

COPY --from=client-builder /compose/docker-compose.yaml .
COPY metadata.json .
COPY faros.svg .
COPY --from=client-builder /ui/dist ui
