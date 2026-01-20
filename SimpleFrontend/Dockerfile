# Stage 1: Build Stage
# --------------------
FROM node:20-alpine AS build
WORKDIR /app
RUN corepack enable && corepack prepare pnpm@latest --activate
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile
COPY . .
RUN pnpm build

# -----------------------------------------------------------------
# Stage 2: Production Stage ( Using Nginx )
# -----------------------------------------
FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html
RUN rm -rf ./*
# Copy the build output from the previous stage
COPY --from=build /app/dist .
# Copy our custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]