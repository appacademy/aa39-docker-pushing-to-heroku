# Use a build environment to build the React application within the image 
# rather than needing a pre-built app

# BUILD ENVIRONMENT

# Match node to your version
FROM node:12-alpine as build

# Base folder to create and use within the container
WORKDIR /app

# Add the node_modules/.bin folder to the path
ENV PATH /app/node_modules/.bin:$PATH

# Copy the `package.json` files
COPY ./frontend/package.json ./
COPY ./frontend/package-lock.json ./

# Install dependencies
RUN npm ci --silent

# Install the latest v3 of the CRA scripts
RUN npm install react-scripts@^3.0.0 -g --silent

# Copy the frontend files to the container
COPY ./frontend/ ./

# Run the React build script
RUN npm run build

# PRODUCTION ENVIRONMENT

# Match node to your version
FROM node:12-alpine

# Base folder to create and use within the container
WORKDIR /app

# To allow waiting for database
COPY ./wait-for .

# Copy backend code
COPY ./backend .

# Copy already-built frontend React app
COPY --from=build /app/build ./public

# Install dependencies
RUN npm ci --silent

# Define environment variables
ENV NODE_ENV production
ENV JWT_SECRET aaa967f1-2b08-4dde-a086-5df6bc8eff91
ENV JWT_EXPIRES_IN 604800

# Heroku requires a command
CMD npm start
