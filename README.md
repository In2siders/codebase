# In2siders Codebase

Docker Compose configurations to launch the full In2siders project with ease.

## Overview

This repository contains Docker Compose files to run the In2siders application stack, which consists of:
- **Frontend**: React-based web application (`In2siders/frontend-web`)
- **Backend**: Node.js API server (`In2siders/backend`)
- **Database**: Optional PostgreSQL database

## Prerequisites

- Docker Engine (version 20.10 or higher)
- Docker Compose (version 2.0 or higher)
- GitHub Container Registry access for pulling the images

## Quick Start

### Option 1: Local Database (SQLite)

Use this configuration for quick testing with a local SQLite database:

```bash
# Start the services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the services
docker-compose down
```

The application will be available at:
- Frontend: http://localhost:3000
- Backend API: http://localhost:4000

### Option 2: PostgreSQL Database

Use this configuration for a more production-like setup with PostgreSQL:

```bash
# Start the services with PostgreSQL
docker-compose -f docker-compose.db.yml up -d

# View logs
docker-compose -f docker-compose.db.yml logs -f

# Stop the services
docker-compose -f docker-compose.db.yml down

# Stop and remove volumes (clean slate)
docker-compose -f docker-compose.db.yml down -v
```

The application will be available at:
- Frontend: http://localhost:3000
- Backend API: http://localhost:4000
- PostgreSQL: localhost:5432

## Configuration

### Environment Variables

Copy the example environment file and customize as needed:

```bash
cp .env.example .env
```

Available environment variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `BACKEND_URL` | `http://localhost:4000` | Backend API URL for frontend |
| `BACKEND_PORT` | `4000` | Backend server port |
| `NODE_ENV` | `production` | Node environment |
| `POSTGRES_DB` | `in2siders` | PostgreSQL database name |
| `POSTGRES_USER` | `in2siders` | PostgreSQL username |
| `POSTGRES_PASSWORD` | `in2siders` | PostgreSQL password |
| `DATABASE_TYPE` | `sqlite` | Database type (sqlite/postgres) |
| `DATABASE_PATH` | `./data/database.db` | SQLite database path |

## Files

- `docker-compose.yml` - Basic setup with frontend and backend (local SQLite database)
- `docker-compose.db.yml` - Full stack with PostgreSQL database
- `.env.example` - Example environment variables

## Data Persistence

### Local Database Mode
- SQLite database is stored in a Docker volume (`backend-data`)
- Data persists between container restarts

### PostgreSQL Mode
- PostgreSQL data is stored in a Docker volume (`postgres-data`)
- Data persists between container restarts
- To completely reset: `docker-compose -f docker-compose.db.yml down -v`

## Troubleshooting

### Port Conflicts

If ports 3000, 4000, or 5432 are already in use, you can modify them in your `.env` file or directly in the compose files.

### Image Pull Issues

If you encounter authentication issues pulling images from GitHub Container Registry:

```bash
# Login to GitHub Container Registry
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

### Viewing Logs

```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f backend

# With PostgreSQL setup
docker-compose -f docker-compose.db.yml logs -f database
```

### Health Checks

Check if services are running:

```bash
docker-compose ps
# or
docker-compose -f docker-compose.db.yml ps
```

## Development

For development purposes, you may want to rebuild images:

```bash
# Pull latest images
docker-compose pull

# Restart services
docker-compose up -d
```

## Support

For issues with specific services, please refer to their respective repositories:
- Frontend: [In2siders/frontend-web](https://github.com/In2siders/frontend-web)
- Backend: [In2siders/backend](https://github.com/In2siders/backend)
