```bash
# Stage 1: Build
FROM node:18 AS builder

# Set the working directory
WORKDIR /app

# Copy package files and install dependencies
COPY app/package*.json ./
RUN npm install

# Copy the rest of the application code
COPY app/ ./

# Build the Next.js application
RUN npm run build

# Stage 2: Production Image
FROM node:18 AS runner

# Set the working directory
WORKDIR /app

# Install only production dependencies
COPY app/package*.json ./
RUN npm install --production

# Copy built files from the build stage
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/next.config.js ./

# Expose the port Next.js will run on
EXPOSE 3000

# Start the Next.js application
CMD ["npm", "run", "start"]


```
