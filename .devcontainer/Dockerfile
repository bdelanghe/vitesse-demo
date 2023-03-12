FROM mcr.microsoft.com/devcontainers/typescript-node:0-18

COPY pnpm-lock.yaml ./

# If you patched any package, include patches before running pnpm fetch 
COPY patches patches

RUN pnpm fetch --prod

ADD . ./
RUN pnpm install -r --offline --prod

EXPOSE 3333
CMD [ "pnpm", "dev" ]