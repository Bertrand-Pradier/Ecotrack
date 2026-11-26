FROM node:20-alpine

ENV NODE_ENV=production
WORKDIR /app

# Installation reproductible des dépendances à partir du package-lock.json
COPY package.json package-lock.json ./
RUN npm ci --omit=dev && npm cache clean --force

COPY server.js .

# L'image node fournit déjà un utilisateur non root "node"
USER node

EXPOSE 5000

HEALTHCHECK --interval=10s --timeout=3s --start-period=10s --retries=5 \
  CMD wget -qO- http://127.0.0.1:5000/health || exit 1

CMD ["npm", "start"]
