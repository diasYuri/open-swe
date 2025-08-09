# Production image for LangGraph agent (@open-swe/agent) using Hono HTTP server

FROM node:20-alpine

ENV NODE_ENV=production \
    PORT=2024

WORKDIR /app

RUN apk add --no-cache libc6-compat

# Copy monorepo context
COPY package.json yarn.lock turbo.json tsconfig.json .yarnrc.yml langgraph.json ./
COPY apps ./apps
COPY packages ./packages

RUN corepack enable

# Install all workspace dependencies
RUN yarn install --frozen-lockfile

# Build shared libs and the agent workspace
RUN yarn workspace @open-swe/shared build \
 && yarn workspace @open-swe/agent build

# Expose HTTP port used by langgraph http server
EXPOSE 2024

# Start via LangGraph CLI picking up http.app from langgraph.json
CMD ["npx", "-y", "@langchain/langgraph-cli@latest", "start", "--config", "./langgraph.json", "--port", "2024"]


