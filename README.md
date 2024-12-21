# Docker Development Environment

This repository contains a Docker-based development environment for a Laravel backend and Next.js frontend application with PostgreSQL database.

## Prerequisites

- Docker Engine 24.x or later
- Docker Compose v2.x or later
- Git

## Project Structure

```
docker-apps/
├── backend/               # Laravel application
│   ├── public/           # Laravel public files
│   ├── storage/          # Laravel storage
│   └── docker/          # Docker configuration files
├── frontend/             # Next.js application
├── docker-compose.yml    # Docker services configuration
└── .env.docker          # Docker environment variables
```

## Quick Start

1. Clone the repository:
```bash
git clone <repository-url>
cd docker-apps
```

2. Copy the environment file:
```bash
cp .env.docker .env
```

3. Generate Laravel application key:
```bash
docker-compose run --rm backend php artisan key:generate --show
```
Update the generated key in `.env` file.

4. Build and start the containers:
```bash
docker-compose up --build -d
```

5. Install Laravel dependencies:
```bash
docker-compose exec backend composer install
```

6. Run Laravel migrations:
```bash
docker-compose exec backend php artisan migrate
```

## Accessing Services

- Laravel Backend: http://localhost:8000
- Next.js Frontend: http://localhost:3000
- PostgreSQL Database:
  - Host: localhost
  - Port: 5432
  - Default credentials in `.env.docker`
- pgAdmin: http://localhost:5050
  - Default login: admin@admin.com / admin

## Development Workflow

### Backend (Laravel)

- Laravel files are mounted at `/var/www/html` in the container
- Run artisan commands:
```bash
docker-compose exec backend php artisan <command>
```
- Access Laravel logs:
```bash
docker-compose exec backend tail -f storage/logs/laravel.log
```

### Frontend (Next.js)

- Next.js files are mounted at `/app` in the container
- Install new dependencies:
```bash
docker-compose exec frontend npm install <package-name>
```
- Access frontend logs:
```bash
docker-compose logs -f frontend
```

### Database

- Connect to PostgreSQL:
```bash
docker-compose exec db psql -U postgres -d myapp
```
- Create database backup:
```bash
docker-compose exec db pg_dump -U postgres myapp > backup.sql
```

## Common Commands

```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f [service_name]

# Rebuild containers
docker-compose up --build -d

# Remove all containers and volumes
docker-compose down -v
```

## Troubleshooting

1. **Permission Issues**
   ```bash
   docker-compose exec backend chown -R www-data:www-data storage bootstrap/cache
   ```

2. **Database Connection Issues**
   - Verify database credentials in `.env`
   - Ensure database service is running:
   ```bash
   docker-compose ps db
   ```

3. **Cache Issues**
   ```bash
   docker-compose exec backend php artisan config:clear
   docker-compose exec backend php artisan cache:clear
   ```

## Production Deployment

For production deployment:

1. Update environment variables:
   - Set `APP_ENV=production`
   - Set `APP_DEBUG=false`
   - Use strong passwords
   - Update APP_KEY

2. Build production images:
```bash
docker-compose -f docker-compose.prod.yml build
```

3. Use proper SSL/TLS certificates for production.

## Contributing

1. Create a new branch for your feature
2. Make your changes
3. Submit a pull request

## License

[Your License Here]
