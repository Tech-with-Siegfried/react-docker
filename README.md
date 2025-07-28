# React Docker Example: 5‑minute crash course

This repository accompanies a short Tech with Siegfried YouTube video that teaches the basics of Docker in about five minutes. It includes a very simple React application, a Dockerfile that builds the app into a container image and a command.txt file that lists the commands shown in the video. 

Link to Video: https://www.youtube.com/watch?v=iG-lErIM5hI

## What is Docker?

Docker is the most popular platform for packaging and running applications in containers. Containers are lightweight isolated units that run on top of the host operating system’s kernel, in contrast to virtual machines which run a full guest operating system with its own kernel. Because containers share the underlying kernel but remain sandboxed from other processes, they are far smaller and faster to start than VMs. A single container holds your application code, its dependencies and environment configuration, meaning it will run the same way on your laptop, a colleague’s machine or a production server. This eliminates the classic “it works on my machine” problem and simplifies deployment.

Docker uses a few key building blocks:

| Concept     | Description |
|-------------|-------------|
| **Dockerfile** | A text file with instructions that tell Docker how to build an image of your application. Typical instructions include selecting a base image (FROM), setting a working directory, copying files, installing dependencies and defining the command to run the app. |
| **Image** | A read‑only template built from a Dockerfile. Images are based on a base image (for example, `node:18-alpine`) and capture your app code and dependencies. They can be pulled from a registry such as Docker Hub or built locally. |
| **Container** | A running instance of an image. When you execute `docker run`, Docker creates a container using the image and isolates it from other processes on your host. |

## Repository structure

```
.
├── src/               # React application source code
├── public/            # Static assets for React (HTML template, icons etc.)
├── Dockerfile         # Instructions for building the container image
├── package.json       # NPM dependencies and scripts for the React app
├── command.txt        # Shell commands used in the video
└── README.md          # This readme
```

## command.txt

The `command.txt` file contains the terminal commands demonstrated in the video. Each line is annotated in the video, but having them here makes it easy to copy and paste. The commands include:

- Building the React app image using your Dockerfile (`docker build -t react-test:v1 .`), tagging it with a custom name and version.
- Running the image in detached mode and mapping a host port to the container port (`docker run -d -p 3000:3000 react-test:v1`).
- Rebuilding the image after making changes (`docker build -t react-test:v2 .`) and running the new version on a different port (`docker run -d -p 3001:3000 react-test:v2`). This shows how multiple versions of your app can coexist as separate containers.
- Listing all running docker containers (`docker ps`) and stoping a specific container (`docker stop [CONTAINER-NAME]`)

## Running the React app locally

You can run the React application directly on your machine without Docker. This is useful for development before you containerize it:

Clone this repository:

```bash
git clone https://github.com/your-username/react-docker-example.git
cd react-docker-example
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm start
```

The app should be available at [http://localhost:3000](http://localhost:3000).

## Dockerizing the React app

The provided Dockerfile is based on an official Node.js runtime (`node:18-alpine`) and builds a production version of the React app. Here is a breakdown of each instruction:

- **Base image**: `FROM node:18-alpine` tells Docker to start from a lightweight Alpine Linux image with Node.js preinstalled.
- **Working directory**: `WORKDIR /app` sets the working directory inside the container.
- **Install dependencies**: `COPY package*.json ./` followed by `RUN npm install` copies only the package files and installs dependencies. This step is cached so that dependency installation is not repeated when only your source code changes.
- **Copy application code**: `COPY . .` copies the rest of your source files into the container.
- **Expose port**: `EXPOSE 3000` indicates that the app listens on port 3000.
- **Start command**: `CMD ["npm", "start"]` runs the start script defined in `package.json` when the container launches.

### Build and run the image

Use the following commands from the repository root to build and run your containerized React app:

```bash
# Build the container image and tag it as react-test:v1
docker build -t react-test:v1 .

# Run the container in detached mode (-d) and map your host’s port 3000 to the container’s port 3000
docker run -d -p 3000:3000 react-test:v1
```

Visit [http://localhost:3000](http://localhost:3000) in your browser to see the app running inside a container.

You can iterate on your code by changing files, tagging a new version and running it on a different port. For example:

```bash
docker build -t react-test:v2 .
docker run -d -p 3001:3000 react-test:v2
```

## Feedback and contributions

This repository was created as part of a YouTube tutorial using some AI assistance. Feel free to star the repository if you find it helpful, open an issue if you spot a problem or submit a pull request if you would like to improve the example. Suggestions for future topics whether programming languages, frameworks, DevOps practices or general programming advice are very welcome to be left in the comments of the video.

You can also leave feedback in the comments section of the video. Thank you for watching and see you next time on Tech with Siegfried.
