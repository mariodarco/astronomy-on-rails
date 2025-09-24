# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a Ruby on Rails 8.0 application called `astronomy_on_rails`, built with modern Rails features and deployment tooling. The project uses:

- **Rails 8.0.3** with Ruby 3.4.6
- **SQLite** as the database with Solid Cache, Solid Queue, and Solid Cable for performance
- **Modern asset pipeline** via Propshaft and importmap-rails
- **Hotwire** (Turbo + Stimulus) for SPA-like behavior
- **Kamal** for containerized deployment
- **Docker** for containerization with multi-stage builds

## Development Commands

### Setup and Installation
```bash
# Initial setup (runs bundle install, db setup, and starts server)
bin/setup

# Setup without starting the server
bin/setup --skip-server
```

### Running the Application
```bash
# Start the development server
bin/dev

# Start Rails server directly
bin/rails server

# Access Rails console
bin/rails console

# Database console
bin/rails dbconsole
```

### Testing
```bash
# Run all tests
bin/rails test

# Run system tests only
bin/rails test:system

# Run specific test file
bin/rails test test/models/specific_model_test.rb

# Prepare test database
bin/rails db:test:prepare
```

### Database Operations
```bash
# Setup/reset database
bin/rails db:prepare

# Create and run migrations
bin/rails db:create db:migrate

# Seed database
bin/rails db:seed

# Drop and recreate database
bin/rails db:reset
```

### Code Quality and Security
```bash
# Run RuboCop linter
bin/rubocop

# Auto-fix RuboCop violations
bin/rubocop -A

# Security vulnerability scanning
bin/brakeman

# JavaScript dependency security audit
bin/importmap audit
```

### Deployment (Kamal)
```bash
# Deploy application
bin/kamal deploy

# Setup servers
bin/kamal setup

# Access deployed app console
bin/kamal console

# View deployment logs
bin/kamal logs

# Access shell on deployed container
bin/kamal shell

# Database console on deployed app
bin/kamal dbc
```

## Architecture and Structure

### Rails 8 Modern Stack
This application leverages Rails 8's new solid foundations:
- **Solid Cache**: Database-backed caching instead of Redis for simplicity
- **Solid Queue**: Database-backed job processing built into Puma (SOLID_QUEUE_IN_PUMA=true)
- **Solid Cable**: Database-backed Action Cable for WebSocket connections

### Asset Pipeline
- Uses **Propshaft** (Rails 8's modern asset pipeline) instead of Sprockets
- **Importmap-rails** for JavaScript module management without bundling
- **Hotwire** stack (Turbo + Stimulus) for interactive frontend without complex JavaScript builds

### Deployment Strategy
- **Kamal-based deployment** with Docker containerization
- Configured for single-server deployment with SQLite persistence
- Uses **Thruster** for HTTP caching and compression in production
- **Let's Encrypt SSL** integration via proxy configuration

### Testing Framework
- Standard Rails testing with **Minitest**
- **Capybara + Selenium** for system testing with Chrome
- CI pipeline tests security, linting, and functionality

### Code Style
- **RuboCop Rails Omakase** configuration for consistent Ruby styling
- Follows Rails 8 conventions and modern Ruby practices

## Configuration Notes

### Ruby Version
- Uses Ruby 3.4.6 (specified in `.ruby-version`)
- Bundler caching enabled in CI

### Database Configuration
- SQLite in development with solid_* gems for caching, queues, and cable
- Database files stored in persistent volumes during deployment
- Configured for easy scaling to external databases via environment variables

### Security Features
- **Brakeman** for static security analysis
- **Content Security Policy** configured
- Credential management via Rails encrypted credentials
- Importmap security auditing for JavaScript dependencies

## Development Workflow

When working on this codebase:

1. **Starting development**: Run `bin/setup` for new environments
2. **Daily development**: Use `bin/dev` to start the server
3. **Before committing**: Run `bin/rubocop` and `bin/brakeman`
4. **Testing changes**: Run `bin/rails test` and `bin/rails test:system`
5. **Database changes**: Create migrations with `bin/rails generate migration`

The CI pipeline automatically runs security scans, linting, and tests on every push to main or PR creation.