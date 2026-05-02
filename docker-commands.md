# Build and run the Docker container
docker build -t free-claude-code-guide .
docker run -d -p 8080:8080 --name free-claude-guide free-claude-code-guide

# Access the guide at: http://localhost:8080

# Stop and remove
# docker stop free-claude-guide
# docker rm free-claude-guide
