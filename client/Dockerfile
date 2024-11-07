# Stage 1: Build the application
FROM node:18 AS build

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve the application in production
FROM build AS runner

# Set environment variables for production
ENV NODE_ENV=production

# Set working directory
WORKDIR /app

# Copy the built files from the builder stage
COPY --from=builder /app/next.config.js ./
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./

# Install only production dependencies
RUN npm install --production

# Expose the port the app runs on
EXPOSE 3000

# Start the Next.js application
CMD ["npm", "run", "start"]
