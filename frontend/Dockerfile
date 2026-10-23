FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --registry https://registry.npmmirror.com
COPY . .
RUN npm run build

# nginx 1.31.3 uses a pid-file pwrite path that the CentOS 7 / Docker seccomp
# profile on the 3010 host rejects. Pin the last verified compatible image
# (1.31.2-alpine, digest sha256:54f2a904...) instead of silently widening the
# container's syscall policy. Tag form kept so builds can use a local mirror pull.
FROM nginx:1.31.2-alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf.template /etc/nginx/templates/default.conf.template
EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
