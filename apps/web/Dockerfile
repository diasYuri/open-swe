# Simple monorepo-friendly image for Next.js app @open-swe/web
# This image installs all workspaces (heavier but straightforward) and runs next start

FROM node:20-alpine

ENV NODE_ENV=production \
    PORT=3000 \
    NEXT_TELEMETRY_DISABLED=1

WORKDIR /app

# Often required by Next.js native deps
RUN apk add --no-cache libc6-compat

# Copy monorepo files first to leverage Docker layer caching
COPY package.json yarn.lock turbo.json tsconfig.json .yarnrc.yml ./
COPY apps ./apps
COPY packages ./packages

# Enable Corepack (to use Yarn 3.x as defined in package.json)
RUN corepack enable

# Install dependencies for the entire monorepo (workspace-aware)
RUN yarn install --frozen-lockfile

# Build shared libs that web depends on, then the web workspace
RUN yarn workspace @open-swe/shared build \
 && yarn workspace @open-swe/web build

EXPOSE 3000

# Start Next.js server
CMD ["yarn", "workspace", "@open-swe/web", "start", "-p", "3000"]


