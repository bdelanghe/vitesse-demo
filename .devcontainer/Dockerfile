# [Choice] Node.js version (use -bullseye variants on local arm64/Apple Silicon): 18, 16, 14, 18-bullseye, 16-bullseye, 14-bullseye, 18-buster, 16-buster, 14-buster
ARG VARIANT=18-bullseye
FROM node:${VARIANT}

ENV IMAGE_VARIANT="18-bullseye"

# [Note] Don't use Node 14

ARG USERNAME=node
ARG PNPM_GLOBAL=/usr/local/share/pnpm

# Add PNPM global to PATH.
ENV PATH=${PNPM_GLOBAL}/bin:${PATH}

RUN \
    # Configure global pnpm install location, use group to adapt to UID/GID changes
    if ! cat /etc/group | grep -e "^pnpm:" > /dev/null 2>&1; then groupadd -r pnpm; fi \
    && usermod -a -G pnpm ${USERNAME} \
    && umask 0002 \
    && mkdir -p ${PNPM_GLOBAL} \
    && touch /usr/local/etc/npmrc \
    && chown ${USERNAME}:pnpm ${PNPM_GLOBAL} /usr/local/etc/npmrc \
    && chmod g+s ${PNPM_GLOBAL} \
    && pnpm config -g set prefix ${PNPM_GLOBAL} \
    && su ${USERNAME} -c "pnpm config -g set prefix ${PNPM_GLOBAL}" \
    # Install eslint
    && su ${USERNAME} -c "umask 0002 && pnpm install -g eslint" \
    && su ${USERNAME} -c "umask 0002 && pnpm install -g pnpm" \
    && pnpm cache clean --force > /dev/null 2>&1
