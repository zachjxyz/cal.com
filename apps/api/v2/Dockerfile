FROM node:20-alpine AS build

# Install required packages
RUN apk add --no-cache bash yarn openssl

# Set working directory to root of the monorepo
WORKDIR /calcom

# Build-time environment variables
ARG DATABASE_URL
ARG DATABASE_DIRECT_URL

# Set runtime environment variables
ENV NODE_ENV=production
ENV NODE_OPTIONS=--max-old-space-size=8192

# Copy entire repo
COPY . .

# Install all dependencies from root (for Yarn Workspaces to work)
RUN yarn install

# Generate Prisma client for API
RUN yarn workspace @calcom/api-v2 run generate-schemas

# Build the API
RUN yarn workspace @calcom/api-v2 run build

# Expose correct port for API
EXPOSE 3000

# Start the API in production mode
CMD ["yarn", "workspace", "@calcom/api-v2", "start:prod"]
