# Étape 1 : construction du bundle React avec esbuild
FROM node:20-alpine AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY src ./src
COPY public ./public
RUN npm run build

# Étape 2 : image finale légère, sans esbuild ni sources
FROM node:20-alpine
ENV NODE_ENV=production
WORKDIR /app
COPY --from=build /app/package.json ./
COPY server.js .
COPY --from=build /app/public ./public
USER node
EXPOSE 3000
HEALTHCHECK --interval=10s --timeout=3s --start-period=5s --retries=5 \
  CMD wget -qO- http://127.0.0.1:3000/ >/dev/null || exit 1
CMD ["npm", "start"]
