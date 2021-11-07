# See https://nextjs.org/docs/deployment#docker-image
FROM node:lts-buster-slim AS base
WORKDIR /app

# Install dependencies only when needed
FROM base AS deps
COPY package*.json ./
RUN npm install

# Rebuild the source code only when needed
FROM base AS build
COPY . ./
COPY --from=deps /app/node_modules ./node_modules
# TODO: run tests
RUN npm run build

# Production image, copy all the files and run next
FROM nginx:1.21.3-alpine AS release
COPY --from=build /app/dist /usr/share/nginx/html
