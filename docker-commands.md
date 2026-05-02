# Build and run the Docker container

```bash
docker build -t free-claude-code-guide .
docker run -d -p 8080:8080 --name free-claude-guide free-claude-code-guide
```

Access the guide at: <https://localhost:8080>

## Stop and remove

```bash
docker stop free-claude-guide
docker rm free-claude-guide
```
