# Stage 1: Build React (Vite) app
FROM node:20-alpine AS build
WORKDIR /app

# Install dependencies
COPY package.json package-lock.json ./
RUN npm ci

# Build-time environment: optionally provide VITE_API_URL as a build-arg in CI
ARG VITE_API_URL=""
ENV VITE_API_URL="${VITE_API_URL}"

# Copy source code and build
# If a .env file is present in the build context it will be included by COPY . .
# If not present we create a simple .env containing VITE_API_URL so build won't fail
COPY . .
RUN if [ ! -f .env ]; then \
			printf 'VITE_API_URL="%s"\n' "$VITE_API_URL" > .env; \
		fi
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Copy custom nginx config
COPY nginx.prod.conf /etc/nginx/conf.d/default.conf

# Copy build output from stage 1
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]