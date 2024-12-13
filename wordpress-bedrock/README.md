# Bedrock WordPress Docker Image

A production-ready Docker image for WordPress using the Bedrock modern development stack.

## Features

- Based on PHP 8.1-FPM
- Optimized for Bedrock WordPress stack
- Redis integration ready
- Multisite support
- Composer-based dependency management
- Security-focused configuration
- Production-optimized PHP settings

## Usage

### Basic Usage

```yaml
version: '3.8'
services:
  wordpress:
    image: fydron/bedrock-wordpress:php8.1-fpm
    environment:
      WORDPRESS_DB_HOST: mariadb
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: your_password
      WORDPRESS_DB_NAME: wordpress
      WP_ENV: production
```

## Required Environment Variables

- `WORDPRESS_DB_HOST`: Database hostname
- `WORDPRESS_DB_USER`: Database username
- `WORDPRESS_DB_PASSWORD`: Database password
- `WORDPRESS_DB_NAME`: Database name

## Optional Environment Variables

- `WP_ENV`: Environment type (development/staging/production)
- `WP_HOME`: Full URL to WordPress home
- `WP_SITEURL`: Full URL to WordPress site
- `WP_REDIS_HOST`: Redis host for object caching
- `WP_REDIS_PORT`: Redis port (default: 6379)

## Multisite Configuration

Additional environment variables for multisite setup:

- WP_ALLOW_MULTISITE: Set to true to enable multisite
- MULTISITE: Set to true after network setup
- SUBDOMAIN_INSTALL: Set to true for subdomain installation
- DOMAIN_CURRENT_SITE: Primary domain for the network
- PATH_CURRENT_SITE: Base path for the network

## Performance Optimizations

This image includes:

Optimized PHP-FPM configuration
OPcache settings for production
Customized PHP memory limits
Redis object caching support

## Security Features

Disabled XML-RPC by default
Secure file permissions
Disabled file editing in admin
Environment-based configuration

## Development

### Building the Image

```bash
docker build -t fydron/bedrock-wordpress:php8.1-fpm .
```

### Testing

```bash
docker compose up -d
```

## License

MIT License

## Support

For issues and feature requests, please use the GitHub issue tracker.

This documentation:

1. Clearly identifies what the image is and its purpose
2. Lists all required and optional configuration
3. Provides usage examples
4. Includes important information about features and security
5. Makes it easy for others to understand how to use and contribute to the project
6. Any suggestions get in touch through [support@edgar-ferreira.com](mailto:support@edgar-ferreira.com)
