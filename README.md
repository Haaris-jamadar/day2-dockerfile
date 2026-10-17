# Day 2 – Dockerfile Deep Dive

## Project Overview
This project demonstrates how to containerize a Node.js HTTP API using Docker. It covers Dockerfile instructions, runtime configuration, image-layer caching, and CMD vs ENTRYPOINT.

## Application Endpoints
- `GET /` – Returns a greeting and application environment.
- `GET /health` – Returns `ok`.

## Dockerfile Instructions
- **FROM:** Selects the base image.
- **WORKDIR:** Sets the working directory to `/app`.
- **COPY:** Copies files into the image.
- **RUN:** Installs production dependencies.
- **ARG:** Defines a build-time argument.
- **ENV:** Sets environment variables.
- **EXPOSE:** Documents port 3000.
- **USER:** Runs the application as a non-root user.
- **CMD:** Defines the default startup command.

## Build and Run
```bash
sudo docker build -t demo-api:v1 .
sudo docker run -d --name api -p 3000:3000 demo-api:v1
curl http://localhost:3000/
curl http://localhost:3000/health
```

## Runtime Configuration
The `GREETING` and `APP_ENV` environment variables can be changed when starting the container.

```bash
sudo docker run --rm -p 3000:3000 \
  -e GREETING="Hi Winter Arc" \
  -e APP_ENV=prod demo-api:v1
```

## Docker Layer Caching
The Dockerfile copies package files before installing dependencies. This allows Docker to reuse the dependency installation layer when only application source code changes.

## CMD vs ENTRYPOINT
The `Dockerfile.greeter` experiment uses:

- `ENTRYPOINT ["echo", "Hello"]`
- `CMD ["World"]`

Default output: `Hello World`

When running `docker run --rm greeter:local DevOps`, the output becomes `Hello DevOps` because the supplied argument replaces the default CMD argument.

## COPY vs ADD
`COPY` copies files from the build context. `ADD` supports additional features, including extracting local tar archives.

## ARG vs ENV
`ARG` is intended for build-time configuration. `ENV` sets environment variables available in the image and at runtime. Do not store secrets in either.

## Production Improvements
- Pin base images to specific digests.
- Use reproducible dependency installation.
- Scan images for vulnerabilities.
- Handle secrets securely.

## Evidence
Include screenshots demonstrating:
1. API and health endpoint responses.
2. Runtime environment variable changes.
3. Docker layer caching.
4. CMD and ENTRYPOINT outputs.
5. Docker image inspection and history.
