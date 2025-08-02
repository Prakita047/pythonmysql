# Dockerfile Documentation

## Overview
This repository contains a **Flask-MySQL** application that is containerized using Docker. The Dockerfile follows a **multi-stage build** approach to keep the final image lightweight, secure, and optimized for production.

---

## Multi-Stage Build Structure
The Dockerfile consists of three stages:
- **Base Stage**: Installs dependencies.
- **Build Stage**: Copies the application code and sets up the environment.
- **Final Production Image**: Creates a lightweight, secure, and production-ready container.

---

## Base Stage (Dependency Installation)
```dockerfile
# ---- Base Stage ----
FROM python:3.9-slim AS base

WORKDIR /app

# Create a non-root user for security
RUN groupadd -r appuser && useradd --no-log-init -r -g appuser appuser

# Set umask to restrict file permissions
RUN umask 077

# Copy requirements file and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir Flask mysql-connector-python gunicorn flask_sqlalchemy flask-bcrypt PyJWT pymysql cryptography
```

### Why `umask 077`?
- **Enhanced Security**: Ensures that newly created files and directories are only accessible by the owner.
- **File Permissions**:
  - Files: `rw-------` (600) → Only the owner can read & write.
  - Directories: `rwx------` (700) → Only the owner can read, write & execute.
- **Isolation**: Prevents unauthorized access by other users in the system.
- **Compliance**: Helps enforce security best practices in multi-user environments.

---

## Build Stage (Application Setup)
```dockerfile
# ---- Build Stage ----
FROM python:3.9-slim AS builder
WORKDIR /app
COPY . .

# Recreate the non-root user
RUN groupadd -r appuser && useradd --no-log-init -r -g appuser appuser

# Copy installed dependencies before switching users
COPY --from=base /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
COPY --from=base /usr/local/bin /usr/local/bin

# Switch to non-root user
USER appuser

# Copy application code securely
COPY --chown=appuser:appuser . .
COPY --chown=appuser:appuser templates/ templates/
```

---

## Final Production Image (Optimized Deployment)
```dockerfile
# ---- Final Production Image ----
FROM python:3.9-slim AS final
WORKDIR /app

# Recreate the non-root user
RUN groupadd -r appuser && useradd --no-log-init -r -g appuser appuser

# Switch to non-root user
USER appuser

# Copy application securely
COPY --from=builder --chown=appuser:appuser /app /app

# Copy installed dependencies
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Expose port for Flask application
EXPOSE 5000

# Run the application using Gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "4", "--timeout", "120", "--access-logfile", "-", "--error-logfile", "-", "app:app"]
```

---

## Best Practices Used
✅ **Multi-Stage Builds** → Reduces image size.
✅ **Non-Root User (`appuser`)** → Prevents privilege escalation.
✅ **Security Enhancements** → Uses `umask 077` and correct file ownership.
✅ **Performance Optimizations** → Uses `--no-cache-dir` when installing dependencies.
✅ **Production-Ready Gunicorn Setup** → Ensures a robust web server.
✅ **Minimal Attack Surface** → The final image is lightweight.
✅ **Improved Caching Strategy** → Separates dependencies for better caching.
✅ **Explicit Dependency Management** → Avoids unnecessary packages.

---

## How to Build and Run the Container

### Build the Docker Image
```sh
docker build -t flask-app .
```

### Run the Container
```sh
docker run -p 5000:5000 flask-app
```

### Access the Application
- Open a browser and visit:  
  **`http://localhost:5000`**

---

## Conclusion
This Dockerfile ensures a **secure, efficient, and production-ready** container for the Flask-MySQL application. It follows best practices to provide scalability, performance, and security. 🚀
