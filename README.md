# Vietnamtourist Project

This repository contains Docker configuration for the Vietnamtourist project, which consists of a Laravel backend and Next.js frontend.

## Repository Structure

The project is split into three repositories:
- [vietnamtourist-docker](https://github.com/your-username/vietnamtourist-docker) (this repo) - Docker configuration
- [vietnamtourist-backend](https://github.com/your-username/vietnamtourist-backend) - Laravel backend
- [vietnamtourist-frontend](https://github.com/your-username/vietnamtourist-frontend) - Next.js frontend

## Getting Started

1. Clone this repository with submodules:
```bash
git clone --recursive https://github.com/your-username/vietnamtourist-docker.git
cd vietnamtourist-docker
```

2. If you've already cloned the project without submodules:
```bash
git submodule init
git submodule update
```

3. Copy environment files:
```bash
cp src/backend/.env.example src/backend/.env
cp src/frontend/.env.example src/frontend/.env
```

4. Start the Docker containers:
```bash
docker-compose up -d
```

## Development

### Backend (Laravel)
- Source code is in `src/backend`
- API runs on http://localhost:80
- To run commands in the Laravel container:
```bash
docker exec -it laravel_app bash
```

### Frontend (Next.js)
- Source code is in `src/frontend`
- App runs on http://localhost:3000
- To run commands in the Next.js container:
```bash
docker exec -it nextjs_app sh
```

## Contributing

1. For Docker configuration changes:
- Make changes in this repository
- Submit PR to `vietnamtourist-docker`

2. For Backend changes:
- Work in `src/backend` directory
- Submit PR to `vietnamtourist-backend`

3. For Frontend changes:
- Work in `src/frontend` directory
- Submit PR to `vietnamtourist-frontend`

## Updating Submodules

To update submodules to their latest versions:
```bash
git submodule update --remote
