# NestJS Backend Development Environment Setup Guide
**Complete Production-Ready Configuration**

---

## Table of Contents
1. [Prerequisites Installation](#prerequisites-installation)
2. [Database Setup (PostgreSQL)](#database-setup-postgresql)
3. [Project Initialization](#project-initialization)
4. [Essential Packages Installation](#essential-packages-installation)
5. [Project Structure Setup](#project-structure-setup)
6. [Environment Configuration](#environment-configuration)
7. [Database Configuration (TypeORM)](#database-configuration-typeorm)
8. [Security Configuration](#security-configuration)
9. [Logging & Monitoring](#logging--monitoring)
10. [Testing Setup](#testing-setup)
11. [Production Optimizations](#production-optimizations)
12. [Deployment Checklist](#deployment-checklist)

---

## 1. Prerequisites Installation

### System Requirements
- **Node.js**: Install LTS version (v18 or v20)
  - Download from nodejs.org
  - Verify installation: `node --version` and `npm --version`
  
- **Git**: For version control
  - Verify: `git --version`

- **PostgreSQL**: Database server
  - Install PostgreSQL (latest stable version)
  - Verify service is running
  - Default port: 5432

- **pgAdmin 4**: Database management tool
  - Install from pgadmin.org
  - Configure connection to local PostgreSQL

- **Code Editor**: VS Code (recommended)
  - Install extensions:
    - ESLint
    - Prettier
    - Thunder Client (API testing)
    - GitLens
    - Docker (if using containers)

- **Postman or Insomnia**: For API testing (alternative to Thunder Client)

### Optional but Recommended
- **Docker & Docker Compose**: For containerization
- **Redis**: For caching and session management
- **NVM (Node Version Manager)**: To manage Node versions

---

## 2. Database Setup (PostgreSQL)

### PostgreSQL Installation Steps
1. Download and install PostgreSQL for your OS
2. During installation:
   - Set superuser (postgres) password
   - Set port (default: 5432)
   - Initialize database cluster

### pgAdmin Setup
1. Launch pgAdmin 4
2. Create a new server connection:
   - **Name**: Local Development
   - **Host**: localhost
   - **Port**: 5432
   - **Username**: postgres
   - **Password**: [your password]

### Create Development Database
1. In pgAdmin, right-click "Databases" → "Create" → "Database"
2. Database settings:
   - **Name**: your_project_name_dev
   - **Owner**: postgres
   - **Encoding**: UTF8
   - **Template**: template0
   - **Collation**: Default
   - **Character type**: Default

### Create Test Database
- Repeat above for test database: `your_project_name_test`

### Database User Setup (Security Best Practice)
1. Create dedicated database user (not using postgres superuser)
2. In pgAdmin SQL Query Tool:
   - Create user with password
   - Grant necessary privileges on your database
   - Limit permissions (principle of least privilege)

---

## 3. Project Initialization

### Install NestJS CLI Globally
```bash
npm install -g @nestjs/cli
```

### Create New Project
```bash
nest new project-name
```

### Choose Package Manager
- npm (default)
- yarn
- pnpm (fastest)

### Navigate to Project
```bash
cd project-name
```

### Initialize Git (if not done)
```bash
git init
git add .
git commit -m "Initial commit"
```

### Create GitHub/GitLab Repository
- Push your initial commit to remote repository

---

## 4. Essential Packages Installation

### Core Dependencies

#### Database & ORM
```bash
npm install @nestjs/typeorm typeorm pg
```
- `@nestjs/typeorm`: NestJS TypeORM integration
- `typeorm`: ORM for TypeScript
- `pg`: PostgreSQL driver

#### Configuration Management
```bash
npm install @nestjs/config
```
- Environment variables management
- Type-safe configuration

#### Validation
```bash
npm install class-validator class-transformer
```
- DTO validation
- Request payload transformation

#### Security Packages
```bash
npm install @nestjs/throttler helmet
```
- `@nestjs/throttler`: Rate limiting
- `helmet`: Security headers

#### Authentication & Authorization
```bash
npm install @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt
npm install -D @types/passport-jwt @types/bcrypt
```
- JWT authentication
- Passport strategies
- Password hashing

#### CORS
```bash
npm install cors
npm install -D @types/cors
```

#### Documentation
```bash
npm install @nestjs/swagger swagger-ui-express
```
- Automatic API documentation
- Swagger UI interface

#### Logging
```bash
npm install winston nest-winston
```
- Advanced logging capabilities
- Log levels and transports

#### Caching (Optional but Recommended)
```bash
npm install @nestjs/cache-manager cache-manager
```
- In-memory caching
- Redis adapter (if using Redis)

#### Health Checks
```bash
npm install @nestjs/terminus
```
- Health check endpoints
- Database connectivity monitoring

#### Compression
```bash
npm install compression
npm install -D @types/compression
```
- Response compression for better performance

#### UUID Generation
```bash
npm install uuid
npm install -D @types/uuid
```

#### Date Handling
```bash
npm install dayjs
```
- Better than moment.js (lighter)

---

### Development Dependencies

#### Testing Enhancement
```bash
npm install -D @nestjs/testing supertest
npm install -D @types/supertest
```

#### Code Quality
```bash
npm install -D eslint prettier eslint-config-prettier eslint-plugin-prettier
```

#### TypeScript Types
```bash
npm install -D @types/node @types/express
```

#### Environment Variables Type Safety
```bash
npm install -D cross-env
```
- Cross-platform environment variables

#### Database Migrations Tool
```bash
npm install -D ts-node
```
- Required for TypeORM CLI

---

## 5. Project Structure Setup

### Create Folder Structure
Organize your project with the following structure:

```
src/
├── common/
│   ├── decorators/
│   ├── guards/
│   ├── interceptors/
│   ├── filters/
│   ├── pipes/
│   ├── middleware/
│   └── constants/
├── config/
│   ├── database.config.ts
│   ├── app.config.ts
│   └── validation.schema.ts
├── modules/
│   ├── auth/
│   │   ├── auth.controller.ts
│   │   ├── auth.service.ts
│   │   ├── auth.module.ts
│   │   ├── dto/
│   │   ├── strategies/
│   │   └── guards/
│   ├── users/
│   │   ├── users.controller.ts
│   │   ├── users.service.ts
│   │   ├── users.module.ts
│   │   ├── dto/
│   │   └── entities/
│   └── [other-modules]/
├── database/
│   ├── migrations/
│   ├── seeds/
│   └── entities/
├── utils/
│   └── helpers.ts
├── app.module.ts
├── app.controller.ts
├── app.service.ts
└── main.ts
```

### Create Base Folders
```bash
mkdir -p src/common/{decorators,guards,interceptors,filters,pipes,middleware,constants}
mkdir -p src/config
mkdir -p src/modules
mkdir -p src/database/{migrations,seeds,entities}
mkdir -p src/utils
```

---

## 6. Environment Configuration

### Create Environment Files

#### .env (for development)
Create in project root with:
```
# Application
NODE_ENV=development
PORT=3000
API_PREFIX=api/v1

# Database
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=your_db_user
DB_PASSWORD=your_db_password
DB_DATABASE=your_project_name_dev
DB_SYNCHRONIZE=false

# JWT
JWT_SECRET=your-super-secret-jwt-key-change-in-production
JWT_EXPIRATION=1h
JWT_REFRESH_SECRET=your-refresh-secret
JWT_REFRESH_EXPIRATION=7d

# Security
THROTTLE_TTL=60
THROTTLE_LIMIT=10

# CORS
CORS_ORIGIN=http://localhost:3000,http://localhost:4200

# Logging
LOG_LEVEL=debug

# Redis (if using)
REDIS_HOST=localhost
REDIS_PORT=6379

# Email (if using)
MAIL_HOST=smtp.example.com
MAIL_PORT=587
MAIL_USER=
MAIL_PASSWORD=
MAIL_FROM=noreply@example.com
```

#### .env.production (for production)
- Same structure as .env
- Use production values
- DB_SYNCHRONIZE=false (ALWAYS!)
- Stronger JWT secrets
- Appropriate CORS origins
- LOG_LEVEL=error or warn

#### .env.test (for testing)
- Separate test database
- Minimal logging

### Environment File Security
1. Add to .gitignore:
```
.env
.env.local
.env.production
.env.test
```

2. Create .env.example (template without sensitive data):
```
NODE_ENV=
PORT=
DB_HOST=
# etc...
```

### Configure Validation Schema
Create `src/config/validation.schema.ts` to validate environment variables at startup

---

## 7. Database Configuration (TypeORM)

### Create TypeORM Configuration
File: `src/config/database.config.ts`

#### Configuration Object Structure
- Connection type: postgres
- Host, port, username, password from environment
- Database name
- Entities path
- Migrations path
- Synchronize (false in production!)
- Logging configuration
- SSL settings (for production)
- Extra connection options (pool size, timeouts)

### Register TypeORM in AppModule
Import `TypeOrmModule.forRootAsync()` with:
- ConfigModule integration
- Dynamic configuration factory
- Environment-based settings

### Create DataSource Configuration
File: `src/database/data-source.ts`
- For TypeORM CLI commands
- Migration generation
- Migration running

### Update package.json Scripts
Add migration scripts:
```json
"migration:generate": "typeorm migration:generate -d src/database/data-source.ts",
"migration:run": "typeorm migration:run -d src/database/data-source.ts",
"migration:revert": "typeorm migration:revert -d src/database/data-source.ts"
```

### Entity Base Class
Create `src/database/entities/base.entity.ts` with:
- Primary generated UUID column
- Created at timestamp
- Updated at timestamp
- Soft delete column (optional)

### TypeORM Best Practices
- Use migrations, not synchronize
- Create indexes for frequently queried columns
- Use relations wisely
- Implement soft deletes
- Use transactions for complex operations
- Enable query logging in development

---

## 8. Security Configuration

### Main.ts Security Setup

#### Enable CORS
Configure with:
- Allowed origins from environment
- Credentials support
- Allowed methods
- Allowed headers

#### Helmet Middleware
Set security headers:
- Content Security Policy
- X-Frame-Options
- X-Content-Type-Options
- Strict-Transport-Security

#### Global Validation Pipe
Configure with:
- whitelist: true (strip unknown properties)
- forbidNonWhitelisted: true (throw error on unknown)
- transform: true (auto-transform payloads)
- transformOptions: { enableImplicitConversion: true }

#### Rate Limiting
Configure ThrottlerModule:
- TTL (time to live)
- Limit (max requests)
- Apply globally or per route

### Password Security
- Use bcrypt for hashing
- Salt rounds: 10-12
- Never store plain text passwords
- Implement password policies (complexity, history)

### JWT Security
- Use strong secrets (min 256-bit)
- Short access token expiration (15min - 1h)
- Longer refresh token expiration (7-30 days)
- Implement token rotation
- Store refresh tokens securely
- Blacklist/revoke tokens when needed

### Input Validation
- Validate all DTOs
- Sanitize inputs
- Use class-validator decorators
- Custom validation pipes for complex logic

### SQL Injection Prevention
- TypeORM query builder (parameterized queries)
- Never concatenate user input to queries
- Use repository methods

### XSS Prevention
- Helmet CSP headers
- Sanitize HTML inputs
- Escape outputs

### CSRF Protection
- Use CSRF tokens for state-changing operations
- SameSite cookie attribute

### API Security Headers
- X-Request-ID (request tracking)
- X-Response-Time
- API versioning headers

---

## 9. Logging & Monitoring

### Winston Logger Setup

#### Configure Winston
Create `src/config/logger.config.ts`:
- Console transport (development)
- File transport (errors, combined)
- Format: timestamp, level, message
- Colorize for development
- JSON format for production

#### Integrate with NestJS
Use nest-winston to replace default logger

#### Log Levels
- error: Critical errors
- warn: Warning messages
- info: General information
- http: HTTP requests
- debug: Detailed debug information

### Request Logging Interceptor
Create interceptor to log:
- Request method and URL
- Request body (excluding sensitive data)
- Response status and time
- User information (if authenticated)

### Error Logging
- Log all exceptions with stack traces
- Context information
- User actions leading to error

### Health Check Endpoints
Configure @nestjs/terminus:
- Database health check
- Memory usage
- Disk space
- External services status

### Performance Monitoring
- Response time logging
- Slow query detection
- Memory leak monitoring
- CPU usage tracking

---

## 10. Testing Setup

### Jest Configuration
NestJS comes with Jest pre-configured

### Update Jest Config
- Coverage thresholds (80%+)
- Test environment: node
- Module name mapper for path aliases
- Setup files for test utilities

### Test Structure
```
test/
├── unit/
│   └── [module].service.spec.ts
├── integration/
│   └── [module].controller.spec.ts
└── e2e/
    └── [feature].e2e-spec.ts
```

### Testing Best Practices
- Write unit tests for services
- Integration tests for controllers
- E2E tests for critical flows
- Mock external dependencies
- Use test database
- Clean up after tests
- Test error scenarios

### Test Coverage
Run coverage: `npm run test:cov`
- Aim for 80%+ coverage
- 100% for critical business logic

### Package.json Test Scripts
```json
"test": "jest",
"test:watch": "jest --watch",
"test:cov": "jest --coverage",
"test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --runInBand",
"test:e2e": "jest --config ./test/jest-e2e.json"
```

---

## 11. Production Optimizations

### Build Configuration

#### TypeScript Compiler Options
- target: ES2021
- module: commonjs
- strict: true
- esModuleInterop: true
- skipLibCheck: true
- removeComments: true

#### Build Command
```bash
npm run build
```
Creates optimized production build in `dist/`

### Performance Optimizations

#### Compression
Enable gzip/brotli compression for responses

#### Caching Strategy
- Implement Redis for distributed caching
- Cache frequently accessed data
- Set appropriate TTL
- Cache invalidation strategy

#### Database Query Optimization
- Use proper indexes
- Eager vs Lazy loading strategy
- Query result caching
- Connection pooling configuration

#### Static File Serving
Configure for static assets if needed

### Process Management

#### PM2 (Recommended)
```bash
npm install -g pm2
```
- Manages Node.js processes
- Auto-restart on crash
- Load balancing
- Monitoring
- Log management

#### Create ecosystem.config.js
Configure:
- App name
- Script path
- Instances (cluster mode)
- Max memory restart
- Environment variables
- Log locations

### Docker Setup (Optional)

#### Create Dockerfile
Multi-stage build:
- Build stage
- Production stage
- Minimal image size
- Non-root user

#### Create .dockerignore
Exclude:
- node_modules
- dist
- .git
- .env

#### Create docker-compose.yml
Services:
- NestJS app
- PostgreSQL
- Redis (if using)
- Networks and volumes

---

## 12. Deployment Checklist

### Pre-Deployment Checks

#### Security Audit
- [ ] All dependencies updated
- [ ] No vulnerabilities (`npm audit`)
- [ ] Secrets in environment variables
- [ ] HTTPS enabled
- [ ] Rate limiting configured
- [ ] CORS properly configured
- [ ] Helmet headers enabled

#### Database
- [ ] Migrations tested
- [ ] Backups configured
- [ ] Connection pool optimized
- [ ] Indexes created
- [ ] Synchronize is FALSE

#### Environment Variables
- [ ] All variables documented
- [ ] Production values set
- [ ] Strong JWT secrets
- [ ] Database credentials secured
- [ ] Third-party API keys secured

#### Code Quality
- [ ] All tests passing
- [ ] Linting passing
- [ ] Code reviewed
- [ ] No console.logs
- [ ] Error handling complete

#### Performance
- [ ] Compression enabled
- [ ] Caching implemented
- [ ] Slow queries optimized
- [ ] Load testing completed

#### Monitoring
- [ ] Health checks working
- [ ] Logging configured
- [ ] Error tracking setup (Sentry, etc.)
- [ ] Performance monitoring (New Relic, etc.)

#### Documentation
- [ ] API documentation (Swagger)
- [ ] README updated
- [ ] Deployment guide
- [ ] Environment variables documented

### Deployment Steps
1. Build application: `npm run build`
2. Run migrations: `npm run migration:run`
3. Start application with PM2
4. Verify health check endpoints
5. Monitor logs for errors
6. Test critical API endpoints
7. Monitor performance metrics

### Post-Deployment
- Monitor error rates
- Check response times
- Verify database connections
- Review logs
- Test critical workflows

---

## Additional Production Packages

### Error Tracking
```bash
npm install @sentry/node
```
- Real-time error tracking
- Stack traces
- User context

### API Rate Limiting (Advanced)
```bash
npm install @nestjs/throttler ioredis
```
- Redis-backed rate limiting
- Distributed rate limiting

### Job Queues
```bash
npm install @nestjs/bull bull
```
- Background job processing
- Scheduled tasks
- Email queues

### File Upload
```bash
npm install @nestjs/platform-express multer
npm install -D @types/multer
```
- Handle file uploads
- Validation

### Email Service
```bash
npm install @nestjs-modules/mailer nodemailer
npm install -D @types/nodemailer
```
- Send transactional emails
- Templates

---

## Quick Reference Commands

### Development
```bash
npm run start:dev       # Start in watch mode
npm run start:debug     # Start with debugger
```

### Building
```bash
npm run build           # Production build
npm run start:prod      # Run production build
```

### Testing
```bash
npm run test            # Run unit tests
npm run test:e2e        # Run E2E tests
npm run test:cov        # Run with coverage
```

### Database
```bash
npm run migration:generate -- src/database/migrations/MigrationName
npm run migration:run
npm run migration:revert
```

### Code Quality
```bash
npm run lint            # Run ESLint
npm run format          # Run Prettier
```

---

## Notes & Best Practices

1. **Always use migrations** - Never use synchronize in production
2. **Environment variables** - Never commit sensitive data
3. **Error handling** - Implement global exception filters
4. **Validation** - Validate all inputs, never trust client data
5. **Logging** - Log errors and important events, not sensitive data
6. **Testing** - Write tests as you develop, not after
7. **Documentation** - Keep Swagger docs updated
8. **Security** - Regular dependency updates and audits
9. **Monitoring** - Set up alerts for critical errors
10. **Backups** - Regular database backups with tested restore process

---

**Document Version**: 1.0  
**Last Updated**: October 2025  
**Framework**: NestJS 10.x  
**Database**: PostgreSQL + TypeORM
