FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci --prefer-offline --no-audit --no-fund --silent

COPY . .

ENV VITE_API_URL=http://localhost:8000
ENV VITE_WS_URL=ws://localhost:8000

RUN npm run build

RUN npm prune --omit=dev

FROM nginx:1.25-alpine-slim

COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 5173

CMD ["nginx", "-g", "daemon off;"]