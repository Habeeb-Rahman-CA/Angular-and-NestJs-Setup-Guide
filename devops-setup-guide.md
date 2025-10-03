# DevOps & Deployment Complete Guide
**Full-Stack Application Deployment - NestJS Backend + Angular Frontend**

---

## Table of Contents
1. [DevOps Prerequisites](#devops-prerequisites)
2. [Version Control Setup](#version-control-setup)
3. [Server Setup & Configuration](#server-setup--configuration)
4. [Database Deployment](#database-deployment)
5. [Backend Deployment (NestJS)](#backend-deployment-nestjs)
6. [Frontend Deployment (Angular)](#frontend-deployment-angular)
7. [Reverse Proxy Setup (Nginx)](#reverse-proxy-setup-nginx)
8. [SSL/TLS Configuration](#ssltls-configuration)
9. [Docker & Containerization](#docker--containerization)
10. [CI/CD Pipeline](#cicd-pipeline)
11. [Monitoring & Logging](#monitoring--logging)
12. [Backup & Recovery](#backup--recovery)
13. [Scaling Strategies](#scaling-strategies)
14. [Security Hardening](#security-hardening)
15. [Performance Optimization](#performance-optimization)
16. [Deployment Checklist](#deployment-checklist)

---

## 1. DevOps Prerequisites

### Essential Tools Installation

#### Git & Version Control
- **Git**: Latest version
  - Verify: `git --version`
  - Configure user: name and email
  - SSH key setup for remote repositories

#### SSH Client
- **OpenSSH** (Linux/Mac - built-in)
- **PuTTY** (Windows alternative)
- Generate SSH keys for server access
- Configure SSH config file for easy access

#### Terminal Tools
- **Linux/Mac**: Terminal, iTerm2, Hyper
- **Windows**: WSL2, Git Bash, PowerShell, Windows Terminal
- **Cross-platform**: Termius (GUI SSH client)

#### Code Editor
- **VS Code** with extensions:
  - Docker
  - Remote - SSH
  - YAML
  - GitLens
  - Kubernetes
  - HashiCorp Terraform

#### Container Tools
- **Docker Desktop**: For Windows/Mac
- **Docker Engine**: For Linux
- **Docker Compose**: Multi-container management
- Verify: `docker --version` and `docker-compose --version`

#### Cloud CLI Tools (Based on Provider)

##### AWS
```bash
# AWS CLI
aws --version
aws configure
```

##### Google Cloud
```bash
# gcloud CLI
gcloud --version
gcloud init
```

##### Azure
```bash
# Azure CLI
az --version
az login
```

##### Digital Ocean
```bash
# doctl
doctl version
doctl auth init
```

#### Infrastructure as Code (IaC)
- **Terraform**: Infrastructure provisioning
- **Ansible**: Configuration management
- **Pulumi**: Alternative to Terraform

#### CI/CD Tools Knowledge
- GitHub Actions (built-in to GitHub)
- GitLab CI/CD (built-in to GitLab)
- Jenkins (self-hosted)
- CircleCI
- Travis CI

#### Monitoring Tools
- **Grafana**: Visualization
- **Prometheus**: Metrics collection
- **ELK Stack**: Logging (Elasticsearch, Logstash, Kibana)
- **Datadog**: All-in-one (paid)
- **New Relic**: APM (paid)

#### Database Tools
- **pgAdmin**: PostgreSQL management
- **DBeaver**: Universal database tool
- **psql**: PostgreSQL command-line
- **pg_dump**: Database backup utility

#### API Testing
- **Postman**: API development and testing
- **Insomnia**: Alternative to Postman
- **curl**: Command-line HTTP client

#### Domain & DNS
- Domain registrar account (Namecheap, GoDaddy, Google Domains)
- DNS management access
- Understanding of DNS records (A, CNAME, TXT, MX)

---

## 2. Version Control Setup

### Git Repository Structure

#### Monorepo vs Multi-Repo

##### Monorepo (Single Repository)
```
project-root/
├── backend/          # NestJS application
├── frontend/         # Angular application
├── docker/           # Docker configurations
├── scripts/          # Deployment scripts
├── .github/          # GitHub Actions workflows
└── README.md
```
**Pros**:
- Single source of truth
- Easier to manage
- Atomic commits across services
- Simpler CI/CD

**Cons**:
- Larger repository size
- More complex access control

##### Multi-Repo (Separate Repositories)
```
project-backend/      # NestJS repository
project-frontend/     # Angular repository
project-infra/        # Infrastructure repository
```
**Pros**:
- Clear separation
- Independent versioning
- Granular access control
- Smaller repositories

**Cons**:
- Complex coordination
- Multiple CI/CD pipelines
- Dependency management

**Recommendation**: Start with monorepo, split later if needed

### Branch Strategy

#### GitFlow (Traditional)
**Branches**:
- `main` or `master`: Production-ready code
- `develop`: Development branch
- `feature/*`: New features
- `hotfix/*`: Production fixes
- `release/*`: Release preparation

**Workflow**:
1. Create feature branch from develop
2. Work on feature
3. Merge back to develop
4. Create release branch
5. Test and fix in release
6. Merge to main and develop
7. Tag release

#### GitHub Flow (Simplified)
**Branches**:
- `main`: Always deployable
- `feature/*`: All changes

**Workflow**:
1. Create feature branch from main
2. Work and commit
3. Open pull request
4. Review and discuss
5. Deploy for testing
6. Merge to main

#### Trunk-Based Development (Modern)
**Branches**:
- `main`: Single branch
- Short-lived feature branches (1-2 days)

**Workflow**:
1. Small, frequent commits to main
2. Feature flags for incomplete features
3. Continuous deployment

**Recommendation**: GitHub Flow for most projects

### Commit Guidelines

#### Conventional Commits
Format: `<type>(<scope>): <subject>`

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance

**Examples**:
```
feat(auth): add JWT authentication
fix(user): resolve profile update bug
docs(readme): update installation steps
chore(deps): update Angular to v18
```

#### Commit Best Practices
- Write clear, descriptive messages
- Keep commits atomic (one logical change)
- Reference issue numbers
- Use present tense
- Explain why, not what

### Git Hooks with Husky

#### Pre-commit Hooks
- Run linting
- Run tests
- Check formatting
- Validate commit message

#### Pre-push Hooks
- Run full test suite
- Check for WIP commits
- Validate branch naming

#### Setup Example
```bash
# Install Husky
npm install -D husky

# Initialize
npx husky init

# Add hooks
npx husky add .husky/pre-commit "npm run lint && npm run test"
```

### Pull Request (PR) Guidelines

#### PR Template
Create `.github/PULL_REQUEST_TEMPLATE.md`:
- Description of changes
- Related issues
- Type of change (bugfix, feature, etc.)
- Testing done
- Checklist (tests pass, docs updated, etc.)

#### PR Review Process
1. Code review by at least one person
2. All tests must pass
3. No merge conflicts
4. Approved by reviewer
5. Squash and merge or rebase

#### Branch Protection Rules
- Require PR reviews
- Require status checks
- No direct pushes to main
- Require up-to-date branches
- Include administrators in rules

### Versioning Strategy

#### Semantic Versioning (SemVer)
Format: `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

**Examples**:
- `1.0.0`: Initial release
- `1.1.0`: New feature added
- `1.1.1`: Bug fix
- `2.0.0`: Breaking change

#### Git Tags
```bash
# Create tag
git tag -a v1.0.0 -m "Release version 1.0.0"

# Push tags
git push origin --tags
```

#### Automated Versioning
Use tools like:
- `standard-version`
- `semantic-release`
- Auto-generate changelog

---

## 3. Server Setup & Configuration

### Choose Hosting Provider

#### Cloud Providers Comparison

##### AWS (Amazon Web Services)
**Services**:
- EC2: Virtual servers
- RDS: Managed databases
- S3: Object storage
- CloudFront: CDN
- Route 53: DNS

**Pros**:
- Most comprehensive
- High availability
- Scalable
- Wide global presence

**Cons**:
- Complex pricing
- Steep learning curve
- Can be expensive

##### Google Cloud Platform (GCP)
**Services**:
- Compute Engine: VMs
- Cloud SQL: Databases
- Cloud Storage: Object storage
- Cloud CDN: Content delivery

**Pros**:
- Good pricing
- Strong in data/ML
- Easy to use

**Cons**:
- Smaller than AWS
- Less services

##### Microsoft Azure
**Services**:
- Virtual Machines
- Azure Database
- Blob Storage
- Azure CDN

**Pros**:
- Enterprise integration
- Hybrid cloud
- Strong Windows support

**Cons**:
- Complex interface
- Pricing complexity

##### DigitalOcean
**Services**:
- Droplets: VMs
- Managed Databases
- Spaces: Object storage

**Pros**:
- Simple pricing
- Easy to use
- Great documentation
- Good for startups

**Cons**:
- Fewer advanced features
- Smaller global presence

##### Linode
Similar to DigitalOcean:
- Simple and affordable
- Good performance
- Developer-friendly

##### Hetzner
**Based in Europe**:
- Very affordable
- Good performance
- Data centers in EU

**Recommendation**: DigitalOcean for simplicity, AWS for scale

### Server Specifications

#### Development/Staging Server
- **CPU**: 1-2 cores
- **RAM**: 2-4 GB
- **Storage**: 25-50 GB SSD
- **Bandwidth**: 2-3 TB
- **Cost**: $12-24/month

#### Production Server (Small to Medium)
- **CPU**: 2-4 cores
- **RAM**: 4-8 GB
- **Storage**: 50-100 GB SSD
- **Bandwidth**: 4-5 TB
- **Cost**: $24-48/month

#### Production Server (Medium to Large)
- **CPU**: 4-8 cores
- **RAM**: 8-16 GB
- **Storage**: 100-200 GB SSD
- **Cost**: $48-96/month

### Server Initial Setup

#### Create Server (Droplet/VM)
1. Choose provider
2. Select region (closest to users)
3. Choose OS: **Ubuntu 22.04 LTS** (recommended)
4. Select server size
5. Add SSH key
6. Create server

#### Initial Server Access
```bash
# SSH into server
ssh root@your-server-ip

# Or with key
ssh -i /path/to/private-key root@your-server-ip
```

#### Update System
```bash
# Update package list
apt update

# Upgrade installed packages
apt upgrade -y

# Remove unnecessary packages
apt autoremove -y
```

#### Create Non-Root User
```bash
# Create user
adduser deploy

# Add to sudo group
usermod -aG sudo deploy

# Copy SSH keys
rsync --archive --chown=deploy:deploy ~/.ssh /home/deploy
```

#### Configure SSH
```bash
# Edit SSH config
nano /etc/ssh/sshd_config
```

**Security settings**:
- `PermitRootLogin no`
- `PasswordAuthentication no`
- `PubkeyAuthentication yes`
- `Port 22` (or custom port for security)

```bash
# Restart SSH service
systemctl restart sshd
```

#### Setup Firewall (UFW)
```bash
# Install UFW
apt install ufw

# Allow SSH
ufw allow 22/tcp

# Allow HTTP
ufw allow 80/tcp

# Allow HTTPS
ufw allow 443/tcp

# Allow PostgreSQL (only from specific IPs)
ufw allow from YOUR_IP to any port 5432

# Enable firewall
ufw enable

# Check status
ufw status
```

#### Install Essential Packages
```bash
# Build tools
apt install -y build-essential

# Curl and wget
apt install -y curl wget

# Git
apt install -y git

# Vim or nano
apt install -y vim

# Htop for monitoring
apt install -y htop

# Unzip
apt install -y unzip
```

### Install Node.js

#### Using NodeSource Repository
```bash
# Add Node.js 20.x repository
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js
apt install -y nodejs

# Verify installation
node --version
npm --version
```

#### Using NVM (Recommended)
```bash
# Install NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Load NVM
source ~/.bashrc

# Install Node.js
nvm install 20
nvm use 20
nvm alias default 20

# Verify
node --version
```

### Install PM2 (Process Manager)
```bash
# Install PM2 globally
npm install -g pm2

# Verify
pm2 --version

# Setup PM2 startup script
pm2 startup

# Save PM2 configuration
pm2 save
```

### Install Nginx (Web Server)
```bash
# Install Nginx
apt install -y nginx

# Start Nginx
systemctl start nginx

# Enable on boot
systemctl enable nginx

# Check status
systemctl status nginx
```

### Install PostgreSQL
Covered in next section

---

## 4. Database Deployment

### PostgreSQL Installation

#### Install on Ubuntu Server
```bash
# Add PostgreSQL repository
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Add repository key
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

# Update package list
apt update

# Install PostgreSQL 15
apt install -y postgresql-15 postgresql-contrib-15

# Verify installation
systemctl status postgresql
```

#### Initial Configuration
```bash
# Switch to postgres user
sudo -i -u postgres

# Access PostgreSQL
psql

# Check version
SELECT version();

# Exit
\q
exit
```

### Database Setup

#### Create Database
```bash
# As postgres user
sudo -u postgres psql

# Create database
CREATE DATABASE your_app_production;

# Create user
CREATE USER your_app_user WITH ENCRYPTED PASSWORD 'strong_password_here';

# Grant privileges
GRANT ALL PRIVILEGES ON DATABASE your_app_production TO your_app_user;

# Grant schema privileges (PostgreSQL 15+)
\c your_app_production
GRANT ALL ON SCHEMA public TO your_app_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO your_app_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO your_app_user;

# Exit
\q
```

#### Configure Remote Access (if needed)

##### Edit postgresql.conf
```bash
nano /etc/postgresql/15/main/postgresql.conf
```

**Change**:
```
listen_addresses = 'localhost'
```
**To**:
```
listen_addresses = '*'
```

##### Edit pg_hba.conf
```bash
nano /etc/postgresql/15/main/pg_hba.conf
```

**Add line** (replace with your IP):
```
host    all             all             YOUR_IP/32          md5
```

##### Restart PostgreSQL
```bash
systemctl restart postgresql
```

### Database Security

#### Strong Passwords
- Use generated passwords (32+ characters)
- Store in environment variables
- Never commit to Git

#### Connection Limits
```bash
# In postgresql.conf
max_connections = 100
```

#### SSL Connections
Enable SSL for production:
- Generate SSL certificates
- Configure PostgreSQL to use SSL
- Update connection strings

#### Regular Updates
```bash
# Update PostgreSQL
apt update
apt upgrade postgresql-15
```

### Database Backup Strategy

#### Automated Backups with Cron

##### Create Backup Script
```bash
# Create script
nano /home/deploy/backup_database.sh
```

**Script content**:
```bash
#!/bin/bash
BACKUP_DIR="/home/deploy/backups"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
DB_NAME="your_app_production"

# Create backup
pg_dump -U postgres $DB_NAME | gzip > $BACKUP_DIR/backup_$TIMESTAMP.sql.gz

# Delete backups older than 7 days
find $BACKUP_DIR -name "backup_*.sql.gz" -mtime +7 -delete
```

##### Make Executable
```bash
chmod +x /home/deploy/backup_database.sh
```

##### Schedule with Cron
```bash
# Edit crontab
crontab -e

# Add daily backup at 2 AM
0 2 * * * /home/deploy/backup_database.sh
```

#### Cloud Backup
- Upload backups to S3, Google Cloud Storage, etc.
- Encrypted backups
- Off-site storage

#### Point-in-Time Recovery (PITR)
- Enable WAL archiving
- Continuous backup
- Restore to any point in time

### Database Monitoring

#### Enable Query Logging
```bash
# In postgresql.conf
logging_collector = on
log_directory = 'pg_log'
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'
log_statement = 'all'  # Log all queries (or 'ddl', 'mod')
log_duration = on
log_min_duration_statement = 1000  # Log slow queries (>1s)
```

#### Monitor Performance
- Use pgAdmin
- Query statistics
- Connection pooling metrics
- Slow query identification

### Database Optimization

#### Indexes
- Create indexes on frequently queried columns
- Composite indexes for multi-column queries
- Regular index maintenance

#### Connection Pooling
- Use connection pooling in application
- pgBouncer for external pooling
- Optimize pool size

#### Regular Maintenance
```bash
# Vacuum database
VACUUM ANALYZE;

# Reindex
REINDEX DATABASE your_app_production;
```

### Managed Database Alternative

#### AWS RDS for PostgreSQL
**Pros**:
- Automated backups
- Automatic updates
- High availability
- Read replicas
- Monitoring built-in

**Cons**:
- More expensive
- Less control

#### Other Managed Options
- Google Cloud SQL
- Azure Database for PostgreSQL
- DigitalOcean Managed Databases
- Heroku Postgres

**Recommendation**: Self-hosted for cost savings, managed for convenience

---

## 5. Backend Deployment (NestJS)

### Prepare Backend for Production

#### Environment Variables
Never commit `.env` files. Use server environment variables.

##### Create .env.production on Server
```bash
# Create directory
mkdir -p /var/www/your-app-backend

# Create .env file
nano /var/www/your-app-backend/.env
```

**Content**:
```
NODE_ENV=production
PORT=3000

# Database
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=your_app_user
DB_PASSWORD=your_strong_password
DB_DATABASE=your_app_production
DB_SYNCHRONIZE=false

# JWT
JWT_SECRET=your_super_long_random_jwt_secret_here_256_bits
JWT_EXPIRATION=1h
JWT_REFRESH_SECRET=your_refresh_secret_256_bits
JWT_REFRESH_EXPIRATION=7d

# CORS
CORS_ORIGIN=https://yourdomain.com,https://www.yourdomain.com

# API
API_PREFIX=api/v1

# Logging
LOG_LEVEL=error

# Redis (if using)
REDIS_HOST=localhost
REDIS_PORT=6379
```

##### Secure .env File
```bash
chmod 600 /var/www/your-app-backend/.env
chown deploy:deploy /var/www/your-app-backend/.env
```

### Build Backend

#### Option 1: Build on Server
```bash
# Clone repository
cd /var/www
git clone https://github.com/yourusername/your-app-backend.git

cd your-app-backend

# Install dependencies
npm ci --production

# Build
npm run build

# The dist folder is created
```

#### Option 2: Build Locally & Deploy
```bash
# Build locally
npm run build

# Upload dist folder and package files
scp -r dist package.json package-lock.json deploy@your-server-ip:/var/www/your-app-backend/

# SSH to server and install production dependencies
ssh deploy@your-server-ip
cd /var/www/your-app-backend
npm ci --production
```

### Run Database Migrations
```bash
# On server
cd /var/www/your-app-backend

# Run migrations
npm run migration:run

# Or with TypeORM CLI
npx typeorm migration:run -d dist/database/data-source.js
```

### Start with PM2

#### Create PM2 Ecosystem File
```bash
nano /var/www/your-app-backend/ecosystem.config.js
```

**Content**:
```javascript
module.exports = {
  apps: [{
    name: 'your-app-backend',
    script: 'dist/main.js',
    instances: 2,  // Or 'max' for all CPU cores
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'production'
    },
    error_file: '/var/log/your-app/error.log',
    out_file: '/var/log/your-app/out.log',
    log_date_format: 'YYYY-MM-DD HH:mm:ss Z',
    merge_logs: true,
    autorestart: true,
    max_memory_restart: '1G',
    watch: false
  }]
};
```

#### Start Application
```bash
# Start with PM2
pm2 start ecosystem.config.js

# Or simple start
pm2 start dist/main.js --name your-app-backend -i 2

# Save PM2 process list
pm2 save

# Setup startup script
pm2 startup
```

#### PM2 Commands
```bash
# View apps
pm2 list

# View logs
pm2 logs your-app-backend

# Monitor
pm2 monit

# Restart
pm2 restart your-app-backend

# Stop
pm2 stop your-app-backend

# Delete
pm2 delete your-app-backend

# Reload (zero-downtime)
pm2 reload your-app-backend
```

### Health Check Endpoint

#### Verify Application
```bash
# Test locally on server
curl http://localhost:3000/api/v1/health

# Should return health check response
```

### Deployment Script

#### Create Deploy Script
```bash
nano /home/deploy/deploy_backend.sh
```

**Script**:
```bash
#!/bin/bash

APP_DIR="/var/www/your-app-backend"
BRANCH="main"

echo "Starting backend deployment..."

# Navigate to app directory
cd $APP_DIR

# Pull latest code
git pull origin $BRANCH

# Install dependencies
npm ci --production

# Build application
npm run build

# Run migrations
npm run migration:run

# Reload PM2
pm2 reload ecosystem.config.js

echo "Backend deployment completed!"
```

#### Make Executable
```bash
chmod +x /home/deploy/deploy_backend.sh
```

### Zero-Downtime Deployment

#### Using PM2 Cluster Mode
- Multiple instances running
- PM2 reloads one by one
- Always one instance available

```bash
pm2 reload your-app-backend
```

#### Blue-Green Deployment
- Two identical environments
- Switch traffic between them
- Rollback if issues

---

## 6. Frontend Deployment (Angular)

### Build Angular for Production

#### Local Build
```bash
# In Angular project
npm run build --configuration production

# Or
ng build --configuration production
```

**Output**: `dist/project-name/browser/` folder

#### Build Optimization Verification
- Check bundle sizes
- Verify minification
- Check source maps (uploaded separately)
- Test build locally

### Deploy to Nginx

#### Create Directory
```bash
# On server
mkdir -p /var/www/your-app-frontend
```

#### Upload Build Files

##### Option 1: SCP
```bash
# From local machine
scp -r dist/project-name/browser/* deploy@your-server-ip:/var/www/your-app-frontend/
```

##### Option 2: Git + Build on Server
```bash
# On server
cd /var/www
git clone https://github.com/yourusername/your-app-frontend.git
cd your-app-frontend
npm ci
npm run build --configuration production
```

##### Option 3: Rsync (Better)
```bash
# From local machine
rsync -avz --delete dist/project-name/browser/ deploy@your-server-ip:/var/www/your-app-frontend/
```

#### Set Permissions
```bash
# On server
chown -R www-data:www-data /var/www/your-app-frontend
chmod -R 755 /var/www/your-app-frontend
```

### Configure Nginx for Angular

#### Create Nginx Configuration
```bash
nano /etc/nginx/sites-available/your-app-frontend
```

**Configuration**:
```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    
    root /var/www/your-app-frontend;
    index index.html;
    
    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml+rss application/javascript application/json;
    
    # Angular routing - all requests to index.html
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache static assets
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    
    # Logging
    access_log /var/log/nginx/your-app-frontend-access.log;
    error_log /var/log/nginx/your-app-frontend-error.log;
}
```

#### Enable Site
```bash
# Create symbolic link
ln -s /etc/nginx/sites-available/your-app-frontend /etc/nginx/sites-enabled/

# Test configuration
nginx -t

# Reload Nginx
systemctl reload nginx
```

### Environment Configuration

#### Angular Environment Files
Build with different environments:
```bash
# Production
ng build --configuration production

# Staging
ng build --configuration staging
```

#### Runtime Environment Variables
For dynamic configuration, use `assets/config.json`:
```json
{
  "apiUrl": "https://api.yourdomain.com",
  "environment": "production"
}
```

Load in Angular:
- Use APP_INITIALIZER
- Load config before app starts
- Use throughout application

### Deployment Script

#### Create Deploy Script
```bash
nano /home/deploy/deploy_frontend.sh
```

**Script**:
```bash
#!/bin/bash

APP_DIR="/var/www/your-app-frontend"
BUILD_DIR="/home/deploy/frontend-build"
BRANCH="main"

echo "Starting frontend deployment..."

# Clone/pull repository
if [ -d "$BUILD_DIR" ]; then
    cd $BUILD_DIR
    git pull origin $BRANCH
else
    git clone https://github.com/yourusername/your-app-frontend.git $BUILD_DIR
    cd $BUILD_DIR
fi

# Install dependencies
npm ci

# Build
npm run build --configuration production

# Backup current version
mv $APP_DIR $APP_DIR.backup.$(date +%Y%m%d_%H%M%S)

# Deploy new version
cp -r dist/project-name/browser $APP_DIR

# Set permissions
chown -R www-data:www-data $APP_DIR
chmod -R 755 $APP_DIR

# Remove old backups (keep last 3)
cd /var/www
ls -t | grep 'your-app-frontend.backup' | tail -n +4 | xargs rm -rf

echo "Frontend deployment completed!"
```

#### Make Executable
```bash
chmod +x /home/deploy/deploy_frontend.sh
```

### CDN Deployment (Alternative)

#### Deploy to CDN
For better performance:
- AWS S3 + CloudFront
- Netlify
- Vercel
- Firebase Hosting

**Benefits**:
- Global distribution
- Faster load times
- Automatic SSL
- Automatic builds

---

## 7. Reverse Proxy Setup (Nginx)

### Nginx as Reverse Proxy

#### Purpose
- Route requests to backend API
- Serve frontend static files
- SSL termination
- Load balancing
- Caching
- Compression

### Backend API Proxy Configuration

#### Update Nginx Config
```bash
nano /etc/nginx/sites-available/your-app
```

**Complete Configuration**:
```nginx
# Upstream backend servers
upstream backend_servers {
    least_conn;  # Load balancing method
    server localhost:3000;
    # Add more if using multiple instances
    # server localhost:3001;
    # server localhost:3002;
}

# Frontend server
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    
    # Frontend root
    root /var/www/your-app-frontend;
    index index.html;
    
    # Gzip compression
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_proxied any;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml+rss application/javascript application/json;
    
    # API proxy
    location /api {
        proxy_pass http://backend_servers;
        proxy_http_version 1.1;
        
        # Headers
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # Timeouts
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
        
        # Buffering
        proxy_buffering on;
        proxy_buffer_size 4k;
        proxy_buffers 8 4k;
        
        # Cache bypass
        proxy_cache_bypass $http_upgrade;
    }
    
    # WebSocket support (if needed)
    location /socket.io {
        proxy_pass http://backend_servers;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
    
    # Frontend - Angular routing
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache static assets
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    
    # Logging
    access_log /var/log/nginx/your-app-access.log;
    error_log /var/log/nginx/your-app-error.log;
}
```

#### Enable Configuration
```bash
# Remove default
rm /etc/nginx/sites-enabled/default

# Enable your config
ln -s /etc/nginx/sites-available/your-app /etc/nginx/sites-enabled/

# Test configuration
nginx -t

# Reload Nginx
systemctl reload nginx
```

### Load Balancing

#### Multiple Backend Instances
If running multiple PM2 instances on different ports:

```nginx
upstream backend_servers {
    least_conn;
    server localhost:3000 weight=1;
    server localhost:3001 weight=1;
    server localhost:3002 weight=1;
    
    # Health check (requires nginx plus or third-party module)
    # check interval=3000 rise=2 fall=3 timeout=1000;
}
```

#### Load Balancing Methods
- **round_robin** (default): Distribute evenly
- **least_conn**: Send to server with least connections
- **ip_hash**: Same client to same server
- **random**: Random distribution

### Caching Configuration

#### Cache API Responses
```nginx
# Define cache path
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=api_cache:10m max_size=1g inactive=60m use_temp_path=off;

server {
    # ... other config
    
    location /api {
        # Cache configuration
        proxy_cache api_cache;
        proxy_cache_key "$scheme$request_method$host$request_uri";
        proxy_cache_valid 200 302 10m;
        proxy_cache_valid 404 1m;
        proxy_cache_bypass $http_cache_control;
        add_header X-Cache-Status $upstream_cache_status;
        
        proxy_pass http://backend_servers;
        # ... other proxy settings
    }
}
```

### Rate Limiting

#### Protect Against DDoS
```nginx
# Define rate limit zone
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;

server {
    # ... other config
    
    location /api {
        # Apply rate limit
        limit_req zone=api_limit burst=20 nodelay;
        limit_req_status 429;
        
        proxy_pass http://backend_servers;
        # ... other proxy settings
    }
}
```

### SSL/TLS (Covered in next section)

---

## 8. SSL/TLS Configuration

### Why SSL/TLS is Essential
- Encrypt data in transit
- Protect user credentials
- Required for modern browsers
- Better SEO ranking
- Trust and credibility
- Required for PWA

### Certificate Options

#### Let's Encrypt (Recommended - Free)
**Pros**:
- Free
- Automated renewal
- Trusted by all browsers
- Easy setup

**Cons**:
- 90-day expiration (auto-renewable)
- Rate limits

#### Commercial SSL Certificates
**Providers**: Comodo, DigiCert, GlobalSign
**Pros**:
- Longer validity
- EV certificates available
- Warranty/insurance

**Cons**:
- Cost ($50-$200+/year)

#### Self-Signed Certificates
**Use only for**:
- Development
- Testing
- Internal applications

**Not for production public websites**

### Install Certbot (Let's Encrypt)

#### Installation on Ubuntu
```bash
# Install Certbot and Nginx plugin
apt install -y certbot python3-certbot-nginx

# Verify installation
certbot --version
```

### Obtain SSL Certificate

#### Automatic Configuration
```bash
# Certbot will configure Nginx automatically
certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Follow prompts:
# 1. Enter email address
# 2. Agree to terms
# 3. Choose to redirect HTTP to HTTPS (recommended)
```

#### Manual Configuration
```bash
# Get certificate only (manual Nginx config)
certbot certonly --nginx -d yourdomain.com -d www.yourdomain.com
```

### Nginx SSL Configuration

#### Updated Nginx Config with SSL
```nginx
# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    return 301 https://$server_name$request_uri;
}

# HTTPS server
server {
    listen 443 ssl http2;
    server_name yourdomain.com www.yourdomain.com;
    
    # SSL certificates
    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
    
    # SSL configuration
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384;
    
    # SSL optimization
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    ssl_stapling on;
    ssl_stapling_verify on;
    
    # HSTS (HTTP Strict Transport Security)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    
    # Rest of your configuration
    root /var/www/your-app-frontend;
    index index.html;
    
    # API proxy
    location /api {
        proxy_pass http://backend_servers;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    
    # Frontend routing
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache static assets
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

#### Test and Reload
```bash
# Test configuration
nginx -t

# Reload Nginx
systemctl reload nginx
```

### Automatic Certificate Renewal

#### Certbot Auto-Renewal
```bash
# Test renewal
certbot renew --dry-run

# Certbot automatically creates a cron job or systemd timer
# Check renewal timer
systemctl list-timers | grep certbot
```

#### Manual Cron Job (if needed)
```bash
# Edit crontab
crontab -e

# Add renewal check twice daily
0 0,12 * * * certbot renew --quiet --post-hook "systemctl reload nginx"
```

### Verify SSL Configuration

#### SSL Labs Test
Visit: https://www.ssllabs.com/ssltest/
- Test your domain
- Should get A+ rating
- Check for vulnerabilities

#### Browser Test
- Visit https://yourdomain.com
- Check padlock icon
- Verify certificate details
- No mixed content warnings

### Update Backend CORS

#### Allow HTTPS Origin
In your NestJS backend `.env`:
```
CORS_ORIGIN=https://yourdomain.com,https://www.yourdomain.com
```

Restart backend after updating.

---

## 9. Docker & Containerization

### Why Docker?

**Benefits**:
- Consistent environments (dev = prod)
- Easy deployment
- Isolation
- Scalability
- Portability
- Microservices architecture

### Install Docker

#### On Ubuntu Server
```bash
# Update package index
apt update

# Install prerequisites
apt install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

# Add Docker repository
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Update package index
apt update

# Install Docker
apt install -y docker-ce

# Verify installation
docker --version

# Add user to docker group
usermod -aG docker deploy

# Log out and back in for group changes
```

#### Install Docker Compose
```bash
# Download Docker Compose
curl -L "https://github.com/docker/compose/releases/download/v2.20.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make executable
chmod +x /usr/local/bin/docker-compose

# Verify
docker-compose --version
```

### Backend Dockerfile

#### Create Dockerfile
In backend root directory:

```dockerfile
# Multi-stage build for smaller image

# Stage 1: Build
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Build application
RUN npm run build

# Stage 2: Production
FROM node:20-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install production dependencies only
RUN npm ci --production

# Copy built application from builder stage
COPY --from=builder /app/dist ./dist

# Expose port
EXPOSE 3000

# Start application
CMD ["node", "dist/main.js"]
```

#### Create .dockerignore
```
node_modules
dist
.env
.env.*
.git
.gitignore
README.md
npm-debug.log
coverage
.vscode
.idea
```

### Frontend Dockerfile

#### Multi-stage Build
In frontend root directory:

```dockerfile
# Stage 1: Build Angular
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Build for production
RUN npm run build -- --configuration production

# Stage 2: Serve with Nginx
FROM nginx:alpine

# Copy built application
COPY --from=builder /app/dist/project-name/browser /usr/share/nginx/html

# Copy custom Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```

#### Nginx Configuration for Frontend
Create `nginx.conf` in frontend root:

```nginx
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;
    
    # Gzip compression
    gzip on;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml+rss application/javascript application/json;
    
    # Angular routing
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache static assets
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

### Docker Compose Configuration

#### Create docker-compose.yml
In project root:

```yaml
version: '3.8'

services:
  # PostgreSQL Database
  postgres:
    image: postgres:15-alpine
    container_name: app-postgres
    restart: always
    environment:
      POSTGRES_USER: ${DB_USERNAME}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_DATABASE}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./backups:/backups
    ports:
      - "5432:5432"
    networks:
      - app-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USERNAME}"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Backend API
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: app-backend
    restart: always
    environment:
      NODE_ENV: production
      DB_HOST: postgres
      DB_PORT: 5432
      DB_USERNAME: ${DB_USERNAME}
      DB_PASSWORD: ${DB_PASSWORD}
      DB_DATABASE: ${DB_DATABASE}
      JWT_SECRET: ${JWT_SECRET}
      JWT_EXPIRATION: ${JWT_EXPIRATION}
    ports:
      - "3000:3000"
    depends_on:
      postgres:
        condition: service_healthy
    networks:
      - app-network
    volumes:
      - ./logs:/app/logs

  # Frontend
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: app-frontend
    restart: always
    ports:
      - "80:80"
    depends_on:
      - backend
    networks:
      - app-network

  # Redis (Optional - for caching)
  redis:
    image: redis:7-alpine
    container_name: app-redis
    restart: always
    ports:
      - "6379:6379"
    networks:
      - app-network
    volumes:
      - redis_data:/data

  # Nginx (Reverse Proxy - Optional if not using frontend container's nginx)
  nginx:
    image: nginx:alpine
    container_name: app-nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/conf.d:/etc/nginx/conf.d
      - /etc/letsencrypt:/etc/letsencrypt
      - ./frontend/dist:/usr/share/nginx/html
    depends_on:
      - backend
      - frontend
    networks:
      - app-network

volumes:
  postgres_data:
  redis_data:

networks:
  app-network:
    driver: bridge
```

#### Environment Variables for Docker Compose
Create `.env` file in project root:

```
DB_USERNAME=your_app_user
DB_PASSWORD=your_strong_password
DB_DATABASE=your_app_production
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRATION=1h
```

### Docker Commands

#### Build and Run
```bash
# Build images
docker-compose build

# Start services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Restart service
docker-compose restart backend

# View running containers
docker ps

# Execute command in container
docker exec -it app-backend sh

# View logs of specific service
docker-compose logs -f backend
```

#### Database Migrations in Docker
```bash
# Run migrations
docker-compose exec backend npm run migration:run

# Generate migration
docker-compose exec backend npm run migration:generate -- -n MigrationName
```

### Docker Registry

#### Push to Docker Hub
```bash
# Login
docker login

# Tag image
docker tag app-backend:latest yourusername/app-backend:latest

# Push
docker push yourusername/app-backend:latest
```

#### Private Registry (AWS ECR, Google Container Registry)
- Push to private registry
- More secure
- Used in production deployments

### Docker Security Best Practices

#### Image Security
- Use official base images
- Use specific versions (not `latest`)
- Scan for vulnerabilities
- Multi-stage builds (smaller images)
- Run as non-root user

#### Container Security
- Limit resources (CPU, memory)
- Use read-only file systems where possible
- Drop unnecessary capabilities
- Use secrets management

#### Network Security
- Use private networks
- Limit exposed ports
- Use firewall rules

---

## 10. CI/CD Pipeline

### CI/CD Overview

**Continuous Integration (CI)**:
- Automated testing
- Build verification
- Code quality checks
- Merge validation

**Continuous Deployment (CD)**:
- Automated deployment
- Environment promotion
- Rollback capability
- Zero-downtime deployment

### GitHub Actions (Recommended)

#### Why GitHub Actions?
- Integrated with GitHub
- Free for public repos
- 2000 free minutes/month for private repos
- Easy to configure
- Matrix builds
- Rich marketplace

### Backend CI/CD Pipeline

#### Create Workflow File
`.github/workflows/backend-ci-cd.yml`:

```yaml
name: Backend CI/CD

on:
  push:
    branches: [ main, develop ]
    paths:
      - 'backend/**'
      - '.github/workflows/backend-ci-cd.yml'
  pull_request:
    branches: [ main ]
    paths:
      - 'backend/**'

jobs:
  # Job 1: Test and Build
  test-and-build:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [18.x, 20.x]
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
          cache-dependency-path: backend/package-lock.json
      
      - name: Install dependencies
        working-directory: ./backend
        run: npm ci
      
      - name: Run linter
        working-directory: ./backend
        run: npm run lint
      
      - name: Run tests
        working-directory: ./backend
        run: npm run test
      
      - name: Run test coverage
        working-directory: ./backend
        run: npm run test:cov
      
      - name: Upload coverage reports
        uses: codecov/codecov-action@v3
        with:
          files: ./backend/coverage/lcov.info
          flags: backend
      
      - name: Build application
        working-directory: ./backend
        run: npm run build
      
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: backend-build
          path: backend/dist

  # Job 2: Security Scan
  security-scan:
    runs-on: ubuntu-latest
    needs: test-and-build
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Run npm audit
        working-directory: ./backend
        run: npm audit --audit-level=moderate
      
      - name: Run Snyk security scan
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        with:
          args: --severity-threshold=high

  # Job 3: Deploy to Staging
  deploy-staging:
    runs-on: ubuntu-latest
    needs: [test-and-build, security-scan]
    if: github.ref == 'refs/heads/develop'
    environment: staging
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: backend-build
          path: backend/dist
      
      - name: Deploy to staging server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USERNAME }}
          key: ${{ secrets.STAGING_SSH_KEY }}
          script: |
            cd /var/www/backend-staging
            git pull origin develop
            npm ci --production
            npm run build
            npm run migration:run
            pm2 reload backend-staging

  # Job 4: Deploy to Production
  deploy-production:
    runs-on: ubuntu-latest
    needs: [test-and-build, security-scan]
    if: github.ref == 'refs/heads/main'
    environment: production
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: backend-build
          path: backend/dist
      
      - name: Create deployment package
        run: |
          cd backend
          tar -czf ../backend-deployment.tar.gz dist package.json package-lock.json
      
      - name: Deploy to production server
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USERNAME }}
          key: ${{ secrets.PROD_SSH_KEY }}
          source: "backend-deployment.tar.gz"
          target: "/tmp"
      
      - name: Extract and restart application
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USERNAME }}
          key: ${{ secrets.PROD_SSH_KEY }}
          script: |
            cd /var/www/backend-production
            tar -xzf /tmp/backend-deployment.tar.gz
            npm ci --production
            npm run migration:run
            pm2 reload backend-production --update-env
            rm /tmp/backend-deployment.tar.gz
      
      - name: Health check
        run: |
          sleep 10
          curl -f https://api.yourdomain.com/health || exit 1
      
      - name: Notify deployment
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Backend deployed to production'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
        if: always()
```

### Frontend CI/CD Pipeline

#### Create Workflow File
`.github/workflows/frontend-ci-cd.yml`:

```yaml
name: Frontend CI/CD

on:
  push:
    branches: [ main, develop ]
    paths:
      - 'frontend/**'
      - '.github/workflows/frontend-ci-cd.yml'
  pull_request:
    branches: [ main ]
    paths:
      - 'frontend/**'

jobs:
  # Job 1: Test and Build
  test-and-build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'
          cache-dependency-path: frontend/package-lock.json
      
      - name: Install dependencies
        working-directory: ./frontend
        run: npm ci
      
      - name: Run linter
        working-directory: ./frontend
        run: npm run lint
      
      - name: Run tests
        working-directory: ./frontend
        run: npm run test -- --watch=false --browsers=ChromeHeadless --code-coverage
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./frontend/coverage/lcov.info
          flags: frontend
      
      - name: Build for staging
        if: github.ref == 'refs/heads/develop'
        working-directory: ./frontend
        run: npm run build -- --configuration staging
      
      - name: Build for production
        if: github.ref == 'refs/heads/main'
        working-directory: ./frontend
        run: npm run build -- --configuration production
      
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: frontend-build
          path: frontend/dist

  # Job 2: Deploy to Staging
  deploy-staging:
    runs-on: ubuntu-latest
    needs: test-and-build
    if: github.ref == 'refs/heads/develop'
    environment: staging
    
    steps:
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: frontend-build
          path: frontend/dist
      
      - name: Deploy to staging server
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USERNAME }}
          key: ${{ secrets.STAGING_SSH_KEY }}
          source: "frontend/dist/*"
          target: "/var/www/frontend-staging"
          strip_components: 2
      
      - name: Set permissions
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USERNAME }}
          key: ${{ secrets.STAGING_SSH_KEY }}
          script: |
            chown -R www-data:www-data /var/www/frontend-staging
            chmod -R 755 /var/www/frontend-staging

  # Job 3: Deploy to Production
  deploy-production:
    runs-on: ubuntu-latest
    needs: test-and-build
    if: github.ref == 'refs/heads/main'
    environment: production
    
    steps:
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: frontend-build
          path: frontend/dist
      
      - name: Deploy to production server
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USERNAME }}
          key: ${{ secrets.PROD_SSH_KEY }}
          source: "frontend/dist/*"
          target: "/var/www/frontend-production"
          strip_components: 2
      
      - name: Set permissions and clear cache
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USERNAME }}
          key: ${{ secrets.PROD_SSH_KEY }}
          script: |
            chown -R www-data:www-data /var/www/frontend-production
            chmod -R 755 /var/www/frontend-production
            # Clear Nginx cache if using
            rm -rf /var/cache/nginx/*
            systemctl reload nginx
      
      - name: Notify deployment
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          text: 'Frontend deployed to production'
          webhook_url: ${{ secrets.SLACK_WEBHOOK }}
        if: always()
```

### Docker-Based CI/CD

#### Build and Push Docker Images
`.github/workflows/docker-ci-cd.yml`:

```yaml
name: Docker CI/CD

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      
      - name: Build and push backend image
        uses: docker/build-push-action@v4
        with:
          context: ./backend
          push: true
          tags: |
            yourusername/app-backend:latest
            yourusername/app-backend:${{ github.sha }}
          cache-from: type=registry,ref=yourusername/app-backend:latest
          cache-to: type=inline
      
      - name: Build and push frontend image
        uses: docker/build-push-action@v4
        with:
          context: ./frontend
          push: true
          tags: |
            yourusername/app-frontend:latest
            yourusername/app-frontend:${{ github.sha }}
          cache-from: type=registry,ref=yourusername/app-frontend:latest
          cache-to: type=inline
  
  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.PROD_HOST }}
          username: ${{ secrets.PROD_USERNAME }}
          key: ${{ secrets.PROD_SSH_KEY }}
          script: |
            cd /var/www/your-app
            docker-compose pull
            docker-compose up -d
            docker system prune -f
```

### GitHub Secrets Setup

#### Add Secrets to Repository
1. Go to repository Settings
2. Click on "Secrets and variables" → "Actions"
3. Add repository secrets:
   - `STAGING_HOST`: Staging server IP
   - `STAGING_USERNAME`: SSH username
   - `STAGING_SSH_KEY`: Private SSH key
   - `PROD_HOST`: Production server IP
   - `PROD_USERNAME`: SSH username
   - `PROD_SSH_KEY`: Private SSH key
   - `DOCKERHUB_USERNAME`: Docker Hub username
   - `DOCKERHUB_TOKEN`: Docker Hub access token
   - `SNYK_TOKEN`: Snyk security token
   - `SLACK_WEBHOOK`: Slack webhook URL

### GitLab CI/CD Alternative

#### .gitlab-ci.yml Example
```yaml
stages:
  - test
  - build
  - deploy

variables:
  NODE_VERSION: "20"

# Backend tests
backend-test:
  stage: test
  image: node:$NODE_VERSION
  script:
    - cd backend
    - npm ci
    - npm run lint
    - npm run test
  coverage: '/Statements\s*:\s*(\d+\.?\d*)%/'

# Frontend tests
frontend-test:
  stage: test
  image: node:$NODE_VERSION
  script:
    - cd frontend
    - npm ci
    - npm run lint
    - npm run test -- --watch=false --browsers=ChromeHeadless

# Backend build
backend-build:
  stage: build
  image: node:$NODE_VERSION
  script:
    - cd backend
    - npm ci
    - npm run build
  artifacts:
    paths:
      - backend/dist
    expire_in: 1 hour

# Frontend build
frontend-build:
  stage: build
  image: node:$NODE_VERSION
  script:
    - cd frontend
    - npm ci
    - npm run build -- --configuration production
  artifacts:
    paths:
      - frontend/dist
    expire_in: 1 hour

# Deploy to production
deploy-production:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache openssh-client
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
  script:
    - scp -r backend/dist/* $DEPLOY_USER@$DEPLOY_HOST:/var/www/backend/
    - scp -r frontend/dist/* $DEPLOY_USER@$DEPLOY_HOST:/var/www/frontend/
    - ssh $DEPLOY_USER@$DEPLOY_HOST "cd /var/www/backend && npm ci --production && pm2 reload backend"
  only:
    - main
  environment:
    name: production
    url: https://yourdomain.com
```

### Jenkins Pipeline Alternative

#### Jenkinsfile Example
```groovy
pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS 20'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Backend Tests') {
            steps {
                dir('backend') {
                    sh 'npm ci'
                    sh 'npm run lint'
                    sh 'npm run test'
                }
            }
        }
        
        stage('Frontend Tests') {
            steps {
                dir('frontend') {
                    sh 'npm ci'
                    sh 'npm run lint'
                    sh 'npm run test -- --watch=false --browsers=ChromeHeadless'
                }
            }
        }
        
        stage('Build') {
            parallel {
                stage('Build Backend') {
                    steps {
                        dir('backend') {
                            sh 'npm run build'
                        }
                    }
                }
                stage('Build Frontend') {
                    steps {
                        dir('frontend') {
                            sh 'npm run build -- --configuration production'
                        }
                    }
                }
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sshagent(['production-ssh-key']) {
                    sh '''
                        scp -r backend/dist/* deploy@server:/var/www/backend/
                        scp -r frontend/dist/* deploy@server:/var/www/frontend/
                        ssh deploy@server "cd /var/www/backend && npm ci --production && pm2 reload backend"
                    '''
                }
            }
        }
    }
    
    post {
        success {
            slackSend color: 'good', message: "Deployment successful: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
        failure {
            slackSend color: 'danger', message: "Deployment failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}"
        }
    }
}
```

### Deployment Strategies

#### Blue-Green Deployment
- Two identical environments (blue and green)
- Deploy to inactive environment
- Test thoroughly
- Switch traffic to new environment
- Keep old environment for quick rollback

**Implementation**:
- Two PM2 instances or servers
- Load balancer to switch traffic
- Zero downtime
- Easy rollback

#### Canary Deployment
- Deploy to small subset of users
- Monitor metrics
- Gradually increase traffic
- Rollback if issues detected

**Implementation**:
- Route 10% traffic to new version
- Monitor for 1-2 hours
- Increase to 50%, then 100%
- Use load balancer or feature flags

#### Rolling Deployment
- Update instances one by one
- Always some instances available
- Zero downtime
- Built-in to PM2 cluster mode

```bash
pm2 reload app --update-env
```

---

## 11. Monitoring & Logging

### Why Monitoring is Critical
- Detect issues before users do
- Performance optimization
- Capacity planning
- Debug production issues
- Compliance and auditing
- Understanding user behavior

### Application Monitoring

#### PM2 Monitoring (Built-in)
```bash
# Real-time monitoring
pm2 monit

# Process list with stats
pm2 list

# Detailed info
pm2 show backend

# Logs
pm2 logs backend

# Flush logs
pm2 flush
```

#### PM2 Plus (Cloud Monitoring)
- Free tier available
- Web dashboard
- Real-time metrics
- Exception tracking
- Custom metrics

**Setup**:
```bash
pm2 link [secret-key] [public-key]
```

### Server Monitoring

#### Install Monitoring Tools

##### Prometheus & Grafana
**Prometheus** - Metrics collection
**Grafana** - Visualization dashboard

**Installation**:
```bash
# Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.45.0/prometheus-2.45.0.linux-amd64.tar.gz
tar xvfz prometheus-*.tar.gz
cd prometheus-*

# Create systemd service
sudo nano /etc/systemd/system/prometheus.service
```

**Prometheus service file**:
```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/

[Install]
WantedBy=multi-user.target
```

**Configure Prometheus**:
```yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
  
  - job_name: 'nginx'
    static_configs:
      - targets: ['localhost:9113']
  
  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:9187']
```

##### Node Exporter (System Metrics)
```bash
# Download and install
wget https://github.com/prometheus/node_exporter/releases/download/v1.6.0/node_exporter-1.6.0.linux-amd64.tar.gz
tar xvfz node_exporter-*.tar.gz
sudo cp node_exporter-*/node_exporter /usr/local/bin/

# Create systemd service
sudo nano /etc/systemd/system/node_exporter.service
```

**Service file**:
```ini
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

**Start services**:
```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
```

##### Grafana Installation
```bash
# Add Grafana repository
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

# Install
sudo apt-get update
sudo apt-get install grafana

# Start service
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

**Access**: http://your-server:3000
**Default**: admin/admin

**Configure Data Source**:
1. Add Prometheus data source
2. URL: http://localhost:9090
3. Save and test

**Import Dashboards**:
- Node Exporter Dashboard (ID: 1860)
- Nginx Dashboard (ID: 12708)
- PostgreSQL Dashboard (ID: 9628)

#### Netdata (Alternative - Simpler)
```bash
# One-line installation
bash <(curl -Ss https://my-netdata.io/kickstart.sh)

# Access
http://your-server:19999
```

**Features**:
- Real-time monitoring
- Beautiful UI
- Auto-detection
- Low overhead
- Free and open-source

### Application Performance Monitoring (APM)

#### Sentry (Error Tracking)
**Best for**: Exception tracking and error monitoring

**Backend Setup (NestJS)**:
```bash
npm install @sentry/node
```

**Configure in main.ts**:
```typescript
import * as Sentry from '@sentry/node';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
});
```

**Frontend Setup (Angular)**:
```bash
npm install @sentry/angular
```

**Configure in main.ts**:
```typescript
import * as Sentry from '@sentry/angular';

Sentry.init({
  dsn: 'your-dsn',
  environment: 'production',
  integrations: [
    new Sentry.BrowserTracing(),
    new Sentry.Replay(),
  ],
  tracesSampleRate: 1.0,
  replaysSessionSampleRate: 0.1,
});
```

#### New Relic (Full APM)
**Features**:
- Transaction tracing
- Database monitoring
- External services
- Browser monitoring
- Mobile monitoring

**Installation**:
```bash
npm install newrelic
```

#### Datadog (Enterprise)
**Features**:
- Infrastructure monitoring
- APM
- Log management
- Real user monitoring
- Synthetics

### Logging Strategy

#### Centralized Logging with ELK Stack

##### Elasticsearch + Logstash + Kibana

**Architecture**:
1. Application logs to files
2. Filebeat ships logs to Logstash
3. Logstash processes and sends to Elasticsearch
4. Kibana visualizes logs

**Installation** (Docker Compose):
```yaml
version: '3'
services:
  elasticsearch:
    image: elasticsearch:8.8.0
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms512m -Xmx512m
    ports:
      - "9200:9200"
    volumes:
      - elasticsearch_data:/usr/share/elasticsearch/data
  
  logstash:
    image: logstash:8.8.0
    ports:
      - "5044:5044"
    volumes:
      - ./logstash.conf:/usr/share/logstash/pipeline/logstash.conf
    depends_on:
      - elasticsearch
  
  kibana:
    image: kibana:8.8.0
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch

volumes:
  elasticsearch_data:
```

**Logstash Configuration**:
```conf
input {
  beats {
    port => 5044
  }
}

filter {
  if [type] == "nginx-access" {
    grok {
      match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
  }
  
  if [type] == "app-log" {
    json {
      source => "message"
    }
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "logs-%{+YYYY.MM.dd}"
  }
}
```

##### Filebeat (Log Shipper)
```bash
# Install Filebeat
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.8.0-amd64.deb
sudo dpkg -i filebeat-8.8.0-amd64.deb
```

**Configure Filebeat** (`/etc/filebeat/filebeat.yml`):
```yaml
filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /var/log/nginx/*.log
    fields:
      type: nginx-access
  
  - type: log
    enabled: true
    paths:
      - /var/log/your-app/*.log
    fields:
      type: app-log

output.logstash:
  hosts: ["localhost:5044"]
```

**Start Filebeat**:
```bash
sudo systemctl start filebeat
sudo systemctl enable filebeat
```

#### Alternative: Loki + Grafana
Lighter alternative to ELK:
- Loki: Log aggregation
- Promtail: Log collector
- Grafana: Visualization

### Log Rotation

#### Configure Logrotate
```bash
sudo nano /etc/logrotate.d/your-app
```

**Configuration**:
```
/var/log/your-app/*.log {
    daily
    rotate 14
    compress
    delaycompress
    notifempty
    create 0640 deploy deploy
    sharedscripts
    postrotate
        pm2 reloadLogs
    endscript
}
```

### Alerts and Notifications

#### Alertmanager (with Prometheus)
Configure alerts for:
- High CPU usage (>80%)
- High memory usage (>90%)
- Disk space low (<10%)
- Application downtime
- High error rate
- Slow response times

#### Notification Channels
- **Email**: Critical alerts
- **Slack**: Team notifications
- **PagerDuty**: On-call incidents
- **SMS**: Critical production issues
- **Webhooks**: Custom integrations

### Monitoring Checklist

#### System Metrics
- [ ] CPU usage
- [ ] Memory usage
- [ ] Disk space
- [ ] Disk I/O
- [ ] Network traffic
- [ ] Load average

#### Application Metrics
- [ ] Request rate
- [ ] Response time
- [ ] Error rate
- [ ] Active connections
- [ ] Queue length (if applicable)

#### Database Metrics
- [ ] Connection count
- [ ] Query performance
- [ ] Cache hit ratio
- [ ] Replication lag
- [ ] Disk usage

#### Business Metrics
- [ ] User registrations
- [ ] Active users
- [ ] Conversion rate
- [ ] Revenue (if applicable)

---

## 12. Backup & Recovery

### Backup Strategy

#### 3-2-1 Rule
- **3** copies of data
- **2** different storage media
- **1** off-site backup

### Database Backups

#### Automated PostgreSQL Backups

##### Daily Backup Script
```bash
#!/bin/bash
# /home/deploy/scripts/backup_db.sh

# Configuration
DB_NAME="your_app_production"
DB_USER="postgres"
BACKUP_DIR="/home/deploy/backups/database"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/${DB_NAME}_${TIMESTAMP}.sql.gz"
RETENTION_DAYS=30

# Create backup directory if not exists
mkdir -p $BACKUP_DIR

# Perform backup
pg_dump -U $DB_USER $DB_NAME | gzip > $BACKUP_FILE

# Check if backup was successful
if [ $? -eq 0 ]; then
    echo "Database backup successful: $BACKUP_FILE"
    
    # Delete old backups
    find $BACKUP_DIR -name "${DB_NAME}_*.sql.gz" -mtime +$RETENTION_DAYS -delete
    echo "Old backups deleted (older than $RETENTION_DAYS days)"
else
    echo "Database backup failed!"
    exit 1
fi

# Optional: Upload to cloud storage (S3 example)
# aws s3 cp $BACKUP_FILE s3://your-bucket/database-backups/

# Optional: Send notification
# curl -X POST -H 'Content-type: application/json' \
#   --data '{"text":"Database backup completed successfully"}' \
#   $SLACK_WEBHOOK_URL
```

##### Make Executable and Schedule
```bash
# Make executable
chmod +x /home/deploy/scripts/backup_db.sh

# Test backup
./backup_db.sh

# Schedule with cron (daily at 2 AM)
crontab -e

# Add line:
0 2 * * * /home/deploy/scripts/backup_db.sh >> /var/log/db_backup.log 2>&1
```

#### Point-in-Time Recovery (PITR)

##### Enable WAL Archiving
Edit `/etc/postgresql/15/main/postgresql.conf`:
```
wal_level = replica
archive_mode = on
archive_command = 'test ! -f /var/lib/postgresql/15/archive/%f && cp %p /var/lib/postgresql/15/archive/%f'
```

**Restart PostgreSQL**:
```bash
sudo systemctl restart postgresql
```

### Application Backups

#### Code Repository
- Git repository (GitHub/GitLab) is your backup
- Use tags for releases
- Backup configuration files separately

#### Uploaded Files/Assets
```bash
#!/bin/bash
# /home/deploy/scripts/backup_files.sh

UPLOAD_DIR="/var/www/uploads"
BACKUP_DIR="/home/deploy/backups/files"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/uploads_${TIMESTAMP}.tar.gz"

mkdir -p $BACKUP_DIR

# Create compressed backup
tar -czf $BACKUP_FILE $UPLOAD_DIR

# Delete old backups (keep 14 days)
find $BACKUP_DIR -name "uploads_*.tar.gz" -mtime +14 -delete
```

### Cloud Backup Solutions

#### AWS S3 Backup
```bash
# Install AWS CLI
sudo apt install awscli

# Configure AWS credentials
aws configure

# Upload backup
aws s3 cp /home/deploy/backups/database/latest.sql.gz \
  s3://your-bucket/backups/database/ \
  --storage-class STANDARD_IA

# Lifecycle policy for old backups
# Set in S3 console or via AWS CLI
```

#### Google Cloud Storage
```bash
# Install gsutil
curl https://sdk.cloud.google.com | bash

# Initialize
gcloud init

# Upload backup
gsutil cp /home/deploy/backups/database/latest.sql.gz \
  gs://your-bucket/backups/database/
```

#### DigitalOcean Spaces
```bash
# Install s3cmd
sudo apt install s3cmd

# Configure
s3cmd --configure

# Upload
s3cmd put /home/deploy/backups/database/latest.sql.gz \
  s3://your-space/backups/database/
```

### Restore Procedures

#### Database Restore
```bash
# Restore from compressed backup
gunzip -c backup_file.sql.gz | psql -U postgres -d your_app_production

# Or restore from uncompressed
psql -U postgres -d your_app_production < backup_file.sql
```

#### Application Restore
```bash
# Checkout specific version
git checkout v1.2.3

# Reinstall dependencies
npm ci --production

# Rebuild
npm run build

# Restart
pm2 restart app
```

#### Full System Restore
1. Provision new server
2. Install all dependencies
3. Restore database from backup
4. Deploy application from Git
5. Restore uploaded files
6. Update DNS if needed

### Backup Testing

#### Regular Restore Tests
- Monthly: Test database restore
- Quarterly: Full disaster recovery drill
- Document restore procedures
- Time the restore process

#### Backup Verification
```bash
# Verify backup integrity
gunzip -t backup_file.sql.gz

# Check backup size (should be reasonable)
ls -lh backup_file.sql.gz
```

### Backup Monitoring

#### Check Backup Success
```bash
# Script to verify recent backup exists
#!/bin/bash
BACKUP_DIR="/home/deploy/backups/database"
MAX_AGE_HOURS=25  # Alert if no backup in 25 hours

LATEST_BACKUP=$(find $BACKUP_DIR -name "*.sql.gz" -type f -printf '%T@ %p\n' | sort -n | tail -1 | cut -f2- -d" ")
BACKUP_AGE=$(( ($(date +%s) - $(stat -c %Y "$LATEST_BACKUP")) / 3600 ))

if [ $BACKUP_AGE -gt $MAX_AGE_HOURS ]; then
    echo "ALERT: No recent database backup! Last backup is $BACKUP_AGE hours old"
    # Send alert notification
    exit 1
else
    echo "OK: Recent backup found (${BACKUP_AGE}h old)"
fi
```

---

## 13. Scaling Strategies

### Vertical Scaling (Scale Up)

#### When to Scale Vertically
- Single application instance
- Database performance issues
- Quick solution needed
- Budget constraints

#### How to Scale Vertically
1. Upgrade server resources (CPU, RAM, Disk)
2. Optimize database (indexes, queries)
3. Add caching (Redis)
4. Optimize application code

**Pros**:
- Simple to implement
- No architecture changes
- Cost-effective initially

**Cons**:
- Limited by hardware
- Single point of failure
- Downtime during upgrade

### Horizontal Scaling (Scale Out)

#### When to Scale Horizontally
- High traffic
- Need redundancy
- Better fault tolerance
- Long-term growth

#### Load Balancer Setup

##### Nginx as Load Balancer
```nginx
# /etc/nginx/sites-available/load-balancer

upstream backend_servers {
    least_conn;  # or ip_hash, round_robin
    server 10.0.1.10:3000 weight=3;
    server 10.0.1.11:3000 weight=2;
    server 10.0.1.12:3000 weight=1;
    
    # Health checks (requires nginx plus or module)
    # check interval=3000 rise=2 fall=5 timeout=1000;
}

server {
    listen 80;
    server_name api.yourdomain.com;
    
    location / {
        proxy_pass http://backend_servers;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # Load balancer headers
        add_header X-Upstream-Server $upstream_addr;
    }
}
```

##### HAProxy (Alternative)
More advanced load balancer:
- Better health checks
- More algorithms
- Statistics dashboard
- Advanced routing

##### Cloud Load Balancers
- AWS Elastic Load Balancer (ELB/ALB)
- Google Cloud Load Balancing
- Azure Load Balancer
- DigitalOcean Load Balancer

**Benefits**:
- Managed service
- Auto-scaling integration
- SSL termination
- DDoS protection

### Database Scaling

#### Read Replicas
- Master for writes
- Replicas for reads
- Reduces master load
- Better read performance

**PostgreSQL Replication Setup**:
1. Configure master for replication
2. Create replica servers
3. Configure connection pooling
4. Update application to use read replicas

#### Database Sharding
- Split data across multiple databases
- Horizontal partitioning
- Complex but scalable
- Used by large applications

#### Connection Pooling
**pgBouncer**:
```bash
# Install
sudo apt install pgbouncer

# Configure
sudo nano /etc/pgbouncer/pgbouncer.ini
```

**Configuration**:
```ini
[databases]
your_app = host=localhost port=5432 dbname=your_app_production

[pgbouncer]
listen_addr = 127.0.0.1
listen_port = 6432
auth_type = md5
auth_file = /etc/pgbouncer/userlist.txt
pool_mode = transaction
max_client_conn = 1000
default_pool_size = 25
```

### Caching Strategies

#### Redis Cache
```bash
# Install Redis
sudo apt install redis-server

# Configure
sudo nano /etc/redis/redis.conf
```

**Key Settings**:
```
maxmemory 256mb
maxmemory-policy allkeys-lru
```

**NestJS Redis Integration**:
```bash
npm install @nestjs/cache-manager cache-manager-redis-yet
```

#### CDN for Static Assets
- CloudFlare
- AWS CloudFront
- Fastly
- KeyCDN

**Benefits**:
- Faster load times globally
- Reduced server load
- DDoS protection
- SSL included

### Microservices Architecture

#### When to Consider
- Very large application
- Multiple teams
- Different scaling needs per service
- Complex domain

#### Considerations
- More complex deployment
- Service communication overhead
- Data consistency challenges
- Monitoring complexity

### Auto-Scaling

#### Cloud Auto-Scaling
**AWS Auto Scaling**:
- Define scaling policies
- Scale based on metrics (CPU, memory, requests)
- Min/max instances
- Scheduled scaling

**Kubernetes**:
- Horizontal Pod Autoscaler (HPA)
- Cluster Autoscaler
- Vertical Pod Autoscaler

### Performance Optimization for Scale

#### Application Level
- Implement caching
- Optimize database queries
- Use async processing
- Implement pagination
- Compress responses

#### Database Level
- Add indexes
- Optimize queries
- Use materialized views
- Archive old data
- Partition tables

#### Infrastructure Level
- Use CDN
- Enable compression
- HTTP/2 or HTTP/3
- Keep-alive connections

---

## 14. Security Hardening

### Server Security

#### SSH Hardening
```bash
# Edit SSH config
sudo nano /etc/ssh/sshd_config
```

**Secure Settings**:
```
# Change default port (optional but helps)
Port 2222

# Disable root login
PermitRootLogin no

# Disable password authentication
PasswordAuthentication no
PubkeyAuthentication yes

# Disable empty passwords
PermitEmptyPasswords no

# Limit users
AllowUsers deploy

# Use protocol 2 only
Protocol 2

# Disconnect idle sessions
ClientAliveInterval 300
ClientAliveCountMax 2

# Disable X11 forwarding
X11Forwarding no
```

**Restart SSH**:
```bash
sudo systemctl restart sshd
```

#### Firewall Configuration (UFW)
```bash
# Default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH (use your custom port if changed)
sudo ufw allow 2222/tcp

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow PostgreSQL only from specific IP
sudo ufw allow from YOUR_IP to any port 5432

# Enable firewall
sudo ufw enable

# Check status
sudo ufw status verbose
```

#### Fail2Ban (Brute Force Protection)
```bash
# Install
sudo apt install fail2ban

# Create local config
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

**Configuration**:
```ini
[DEFAULT]
bantime = 3600
findtime = 600
maxretry = 5

[sshd]
enabled = true
port = 2222
logpath = /var/log/auth.log

[nginx-http-auth]
enabled = true

[nginx-limit-req]
enabled = true
logpath = /var/log/nginx/error.log
```

**Start Fail2Ban**:
```bash
sudo systemctl start fail2ban
sudo systemctl enable fail2ban

# Check status
sudo fail2ban-client status
```

### Application Security

#### Environment Variables
- Never commit .env files
- Use strong secrets (256-bit+)
- Rotate secrets regularly
- Use secret management (AWS Secrets Manager, HashiCorp Vault)

#### Input Validation
- Validate all inputs (backend AND frontend)
- Sanitize HTML inputs
- Use parameterized queries (prevent SQL injection)
- Limit file upload sizes and types

#### Authentication Security
- Strong password policies
- Password hashing (bcrypt, rounds 10-12)
- JWT with short expiration
- Refresh token rotation
- Account lockout after failed attempts
- Multi-factor authentication (MFA)

#### API Security
- Rate limiting (prevent DDoS)
- API keys for third-party access
- CORS properly configured
- Helmet.js security headers
- Request size limits
- API versioning

#### Dependencies
```bash
# Check for vulnerabilities
npm audit

# Fix automatically
npm audit fix

# Update dependencies regularly
npm update

# Use Snyk for continuous monitoring
npm install -g snyk
snyk auth
snyk test
```

### Database Security

#### PostgreSQL Security
```bash
# Edit postgresql.conf
sudo nano /etc/postgresql/15/main/postgresql.conf
```

**Settings**:
```
# Listen only on localhost (if app on same server)
listen_addresses = 'localhost'

# SSL connections
ssl = on
ssl_cert_file = '/etc/ssl/certs/server.crt'
ssl_key_file = '/etc/ssl/private/server.key'

# Connection limits
max_connections = 100
```

**pg_hba.conf**:
```
# Local connections
local   all             all                                     peer

# IPv4 local connections (password required)
host    all             all             127.0.0.1/32            scram-sha-256

# SSL-required remote connections
hostssl all             all             0.0.0.0/0               scram-sha-256
```

#### Database User Permissions
- Use principle of least privilege
- Separate users for different apps
- Don't use postgres superuser
- Revoke unnecessary permissions

### SSL/TLS Security

#### Strong Ciphers
Already covered in SSL section, but ensure:
- TLS 1.2 and 1.3 only
- Strong cipher suites
- HSTS enabled
- Perfect Forward Secrecy

#### Certificate Monitoring
- Monitor certificate expiration
- Auto-renewal with Certbot
- Alert before expiration (30 days)

### Security Monitoring

#### Log Monitoring
- Failed login attempts
- Unusual traffic patterns
- Error spikes
- Unauthorized access attempts

#### Security Scanning
- Regular vulnerability scans
- Penetration testing (annually)
- Code security reviews
- Dependency audits

#### Security Tools
- **OWASP ZAP**: Web app security scanner
- **Nmap**: Network scanner
- **Nikto**: Web server scanner
- **Lynis**: Security auditing tool

```bash
# Install Lynis
sudo apt install lynis

# Run audit
sudo lynis audit system
```

### Compliance

#### GDPR (if EU users)
- Data encryption
- Right to be forgotten
- Data portability
- Privacy policy
- Cookie consent

#### PCI DSS (if handling payments)
- Never store CVV
- Encrypt card data
- Regular security audits
- Use payment gateways (Stripe, PayPal)

### Security Checklist

- [ ] Server: SSH key-only authentication
- [ ] Server: Non-standard SSH port
- [ ] Server: Firewall configured (UFW)
- [ ] Server: Fail2Ban installed
- [ ] Server: Regular updates (unattended-upgrades)
- [ ] Server: No root login
- [ ] Application: All inputs validated
- [ ] Application: CORS configured
- [ ] Application: Rate limiting enabled
- [ ] Application: Security headers (Helmet)
- [ ] Database: Strong passwords
- [ ] Database: Limited access (firewall)
- [ ] Database: SSL connections (production)
- [ ] Database: Regular backups
- [ ] SSL: A+ rating on SSL Labs
- [ ] SSL: HSTS enabled
- [ ] Secrets: Not in code
- [ ] Dependencies: No known vulnerabilities
- [ ] Monitoring: Error tracking (Sentry)
- [ ] Monitoring: Log aggregation
- [ ] Backups: Automated and tested
- [ ] Backups: Off-site storage

---

## 15. Performance Optimization

### Backend Performance

#### Node.js Optimization

##### PM2 Cluster Mode
```bash
# Run multiple instances (utilize all CPU cores)
pm2 start dist/main.js -i max

# Or specific number
pm2 start dist/main.js -i 4
```

##### Memory Management
```bash
# Restart if memory exceeds threshold
pm2 start dist/main.js --max-memory-restart 1G
```

##### Node.js Flags
```bash
# Increase memory limit
node --max-old-space-size=4096 dist/main.js

# Enable HTTP/2
# Built into Node.js 8.4+
```

#### Database Query Optimization

##### Indexes
```sql
-- Create indexes on frequently queried columns
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_created_at ON posts(created_at);

-- Composite indexes
CREATE INDEX idx_posts_user_status ON posts(user_id, status);

-- Analyze index usage
SELECT schemaname, tablename, indexname, idx_scan
FROM pg_stat_user_indexes
ORDER BY idx_scan ASC;

-- Remove unused indexes
DROP INDEX IF EXISTS unused_index_name;
```

##### Query Analysis
```sql
-- Explain query execution plan
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';

-- Identify slow queries
SELECT 
  query,
  calls,
  total_time,
  mean_time,
  max_time
FROM pg_stat_statements
ORDER BY mean_time DESC
LIMIT 10;
```

##### Connection Pooling
TypeORM configuration:
```typescript
{
  type: 'postgres',
  host: process.env.DB_HOST,
  // Connection pool settings
  extra: {
    max: 20,              // Maximum connections
    min: 5,               // Minimum connections
    idleTimeoutMillis: 30000,
    connectionTimeoutMillis: 2000,
  }
}
```

#### Caching Implementation

##### Redis Caching
```typescript
// Cache frequently accessed data
@Injectable()
export class UserService {
  async getUserById(id: string) {
    // Check cache first
    const cached = await this.cacheManager.get(`user:${id}`);
    if (cached) return cached;
    
    // Fetch from database
    const user = await this.userRepository.findOne(id);
    
    // Store in cache (TTL: 1 hour)
    await this.cacheManager.set(`user:${id}`, user, 3600);
    
    return user;
  }
}
```

##### HTTP Response Caching
```typescript
// Cache-Control headers
@Controller('products')
export class ProductsController {
  @Get()
  @Header('Cache-Control', 'public, max-age=300') // 5 minutes
  async findAll() {
    return this.productsService.findAll();
  }
}
```

#### API Response Optimization

##### Pagination
Always paginate large datasets:
```typescript
@Get()
async findAll(
  @Query('page') page: number = 1,
  @Query('limit') limit: number = 10,
) {
  const skip = (page - 1) * limit;
  return this.service.findAll({ skip, take: limit });
}
```

##### Select Only Needed Fields
```typescript
// Don't fetch all columns
const users = await this.userRepository.find({
  select: ['id', 'name', 'email'],
});
```

##### Eager vs Lazy Loading
```typescript
// Use lazy loading for relations
const user = await this.userRepository.findOne({
  where: { id },
  relations: ['posts'], // Only load when needed
});
```

#### Compression
Already configured in Nginx, ensure enabled in application:
```typescript
import * as compression from 'compression';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.use(compression());
  await app.listen(3000);
}
```

#### Async Processing

##### Background Jobs with Bull
```bash
npm install @nestjs/bull bull
```

Use queues for:
- Email sending
- Image processing
- Report generation
- Data exports

##### Event-Driven Architecture
```typescript
// Emit events instead of blocking
this.eventEmitter.emit('user.created', { userId: user.id });

// Handle asynchronously
@OnEvent('user.created')
async handleUserCreated(payload: any) {
  await this.emailService.sendWelcomeEmail(payload.userId);
}
```

### Frontend Performance

#### Build Optimization

##### Production Build Analysis
```bash
# Analyze bundle size
npm run build -- --stats-json
npx webpack-bundle-analyzer dist/project-name/stats.json
```

##### Lazy Loading Routes
```typescript
// Lazy load feature modules
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule)
  },
  {
    path: 'dashboard',
    loadChildren: () => import('./dashboard/dashboard.module').then(m => m.DashboardModule)
  }
];
```

##### Code Splitting
Angular does this automatically with lazy loading, but verify:
- Main bundle < 170KB
- Each lazy chunk < 50KB
- Total JavaScript < 500KB

#### Runtime Optimization

##### Change Detection Strategy
```typescript
@Component({
  selector: 'app-user-list',
  changeDetection: ChangeDetectionStrategy.OnPush, // Only check on input changes
  template: `...`
})
export class UserListComponent { }
```

##### Virtual Scrolling
```typescript
import { ScrollingModule } from '@angular/cdk/scrolling';

// In template
<cdk-virtual-scroll-viewport itemSize="50" class="viewport">
  <div *cdkVirtualFor="let item of items">
    {{ item.name }}
  </div>
</cdk-virtual-scroll-viewport>
```

##### TrackBy Function
```typescript
trackByUserId(index: number, user: User) {
  return user.id;
}

// In template
<div *ngFor="let user of users; trackBy: trackByUserId">
  {{ user.name }}
</div>
```

#### Asset Optimization

##### Image Optimization
```bash
# Install imagemin
npm install -D imagemin imagemin-mozjpeg imagemin-pngquant imagemin-webp
```

Best practices:
- Use WebP format with fallbacks
- Lazy load images
- Responsive images (srcset)
- Compress before upload
- Use CDN

##### Font Optimization
```css
/* Preload critical fonts */
<link rel="preload" href="fonts/main.woff2" as="font" type="font/woff2" crossorigin>

/* Use font-display */
@font-face {
  font-family: 'MyFont';
  src: url('fonts/main.woff2') format('woff2');
  font-display: swap; /* Show fallback immediately */
}
```

#### HTTP Optimization

##### HTTP/2
Already enabled with SSL in Nginx

##### Preconnect to APIs
```html
<!-- In index.html -->
<link rel="preconnect" href="https://api.yourdomain.com">
<link rel="dns-prefetch" href="https://api.yourdomain.com">
```

##### Service Worker (PWA)
```bash
# Already added if PWA
ng add @angular/pwa
```

Benefits:
- Offline functionality
- Background sync
- Push notifications
- Faster repeat visits

### Nginx Performance

#### Worker Processes
```nginx
# /etc/nginx/nginx.conf

# Set to number of CPU cores
worker_processes auto;

# Max connections per worker
events {
    worker_connections 2048;
    use epoll; # Linux optimization
    multi_accept on;
}
```

#### Buffering and Timeouts
```nginx
http {
    # Buffer sizes
    client_body_buffer_size 10K;
    client_header_buffer_size 1k;
    client_max_body_size 10M;
    large_client_header_buffers 4 16k;
    
    # Timeouts
    client_body_timeout 12;
    client_header_timeout 12;
    keepalive_timeout 65;
    send_timeout 10;
    
    # File caching
    open_file_cache max=2000 inactive=20s;
    open_file_cache_valid 60s;
    open_file_cache_min_uses 2;
    open_file_cache_errors on;
}
```

#### Gzip Compression
```nginx
http {
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_comp_level 6;
    gzip_types text/plain text/css text/xml text/javascript 
               application/x-javascript application/xml+rss 
               application/javascript application/json 
               image/svg+xml;
    gzip_proxied any;
}
```

#### Static File Caching
```nginx
location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|eot)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
    access_log off;
}
```

### CDN Setup

#### CloudFlare (Free Tier)
1. Sign up at cloudflare.com
2. Add your domain
3. Update nameservers at registrar
4. Configure settings:
   - Auto Minify (JS, CSS, HTML)
   - Brotli compression
   - HTTP/3
   - Rocket Loader
   - Polish (image optimization)

#### AWS CloudFront
1. Create distribution
2. Origin: Your server
3. Configure cache behaviors
4. Set up SSL certificate
5. Update DNS

### Database Performance

#### PostgreSQL Configuration
```bash
sudo nano /etc/postgresql/15/main/postgresql.conf
```

**Optimize for production**:
```ini
# Memory
shared_buffers = 256MB              # 25% of RAM
effective_cache_size = 1GB          # 50-75% of RAM
work_mem = 16MB                     # RAM / max_connections / 4
maintenance_work_mem = 128MB

# Checkpoints
checkpoint_completion_target = 0.9
wal_buffers = 16MB

# Query planning
random_page_cost = 1.1              # For SSDs
effective_io_concurrency = 200      # For SSDs

# Logging
log_min_duration_statement = 1000   # Log slow queries (>1s)
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '
```

**Restart PostgreSQL**:
```bash
sudo systemctl restart postgresql
```

#### Regular Maintenance
```bash
# Create maintenance script
nano /home/deploy/scripts/db_maintenance.sh
```

**Script**:
```bash
#!/bin/bash
DB_NAME="your_app_production"

# Vacuum and analyze
psql -U postgres -d $DB_NAME -c "VACUUM ANALYZE;"

# Reindex
psql -U postgres -d $DB_NAME -c "REINDEX DATABASE $DB_NAME;"

echo "Database maintenance completed"
```

**Schedule weekly**:
```bash
crontab -e

# Sunday at 3 AM
0 3 * * 0 /home/deploy/scripts/db_maintenance.sh
```

### Performance Monitoring

#### Core Web Vitals
- **LCP** (Largest Contentful Paint): < 2.5s
- **FID** (First Input Delay): < 100ms
- **CLS** (Cumulative Layout Shift): < 0.1

#### Tools
- **Lighthouse**: Chrome DevTools
- **WebPageTest**: webpagetest.org
- **GTmetrix**: gtmetrix.com
- **Google PageSpeed Insights**: pagespeed.web.dev

#### Real User Monitoring (RUM)
- Google Analytics 4
- New Relic Browser
- Datadog RUM

---

## 16. Deployment Checklist

### Pre-Deployment Checklist

#### Code & Testing
- [ ] All tests passing (unit, integration, e2e)
- [ ] Code reviewed and approved
- [ ] Linting passing with no warnings
- [ ] No console.log statements
- [ ] No commented-out code
- [ ] Version number updated
- [ ] CHANGELOG.md updated
- [ ] Git tag created

#### Configuration
- [ ] Environment variables set correctly
- [ ] Database connection strings verified
- [ ] API endpoints configured (staging/production)
- [ ] CORS origins updated
- [ ] JWT secrets are strong and unique
- [ ] Third-party API keys configured
- [ ] Feature flags set correctly

#### Security
- [ ] Dependencies have no critical vulnerabilities
- [ ] Security headers configured
- [ ] SSL certificate valid
- [ ] Secrets not in code
- [ ] Input validation implemented
- [ ] Rate limiting configured
- [ ] SQL injection prevention verified
- [ ] XSS protection enabled

#### Database
- [ ] Migrations tested in staging
- [ ] Backup taken before deployment
- [ ] Database indexes optimized
- [ ] Connection pooling configured
- [ ] Slow queries identified and fixed

#### Performance
- [ ] Bundle sizes analyzed and optimized
- [ ] Images optimized
- [ ] Caching implemented
- [ ] CDN configured
- [ ] Compression enabled
- [ ] Lazy loading implemented

#### Infrastructure
- [ ] Server resources adequate
- [ ] Disk space sufficient
- [ ] SSL certificate not expiring soon
- [ ] Firewall rules configured
- [ ] Load balancer configured (if applicable)
- [ ] DNS records correct

#### Monitoring
- [ ] Error tracking configured (Sentry)
- [ ] Application monitoring set up
- [ ] Log aggregation working
- [ ] Alerts configured
- [ ] Uptime monitoring enabled
- [ ] Performance monitoring active

#### Documentation
- [ ] Deployment instructions updated
- [ ] API documentation current
- [ ] README updated
- [ ] Rollback procedure documented
- [ ] Environment variables documented

### Deployment Day Checklist

#### Pre-Deployment
- [ ] Notify team of deployment
- [ ] Schedule maintenance window (if needed)
- [ ] Notify users (if downtime expected)
- [ ] Take full backup (database + files)
- [ ] Verify rollback procedure ready
- [ ] Check monitoring systems

#### During Deployment
- [ ] Deploy to staging first
- [ ] Test staging thoroughly
- [ ] Run smoke tests
- [ ] Deploy to production
- [ ] Monitor deployment logs
- [ ] Watch error rates

#### Post-Deployment
- [ ] Verify application is running
- [ ] Check health check endpoints
- [ ] Test critical user flows
- [ ] Monitor error rates (5-10 minutes)
- [ ] Check performance metrics
- [ ] Verify database migrations applied
- [ ] Test API endpoints
- [ ] Check frontend loads correctly
- [ ] Verify SSL certificate
- [ ] Check logs for errors

#### If Issues Detected
- [ ] Assess severity
- [ ] Check logs and metrics
- [ ] Attempt quick fix if minor
- [ ] Rollback if critical
- [ ] Notify team
- [ ] Document issues
- [ ] Post-mortem after resolution

### Rollback Procedure

#### Quick Rollback Steps
```bash
# 1. Stop current application
pm2 stop backend

# 2. Switch to previous version
cd /var/www/backend
git checkout <previous-tag>  # or use backup

# 3. Reinstall dependencies (if needed)
npm ci --production

# 4. Restore database (if migration was run)
gunzip -c /path/to/backup.sql.gz | psql -U postgres -d your_app_production

# 5. Restart application
pm2 start backend

# 6. Verify rollback successful
curl http://localhost:3000/health
```

#### Database Rollback
```bash
# Revert last migration
npm run migration:revert

# Or restore from backup
psql -U postgres -d your_app_production < backup.sql
```

### Post-Deployment Monitoring

#### First Hour
- Monitor error rates every 5 minutes
- Watch response times
- Check CPU/memory usage
- Review logs for errors
- Verify user reports/support tickets

#### First 24 Hours
- Continue monitoring metrics
- Check error tracking (Sentry)
- Review analytics
- Monitor database performance
- Check backup success

#### First Week
- Daily metric reviews
- Performance comparisons
- User feedback analysis
- Plan optimizations

---

## Maintenance & Operations

### Daily Tasks
- [ ] Check application health
- [ ] Review error logs
- [ ] Monitor server resources
- [ ] Check backup success
- [ ] Review security alerts

### Weekly Tasks
- [ ] Review performance metrics
- [ ] Analyze slow queries
- [ ] Check disk space
- [ ] Review user feedback
- [ ] Update dependencies (minor versions)
- [ ] Database maintenance (vacuum, analyze)

### Monthly Tasks
- [ ] Security audit
- [ ] Full backup test/restore
- [ ] Review and rotate logs
- [ ] Update documentation
- [ ] Performance optimization review
- [ ] Cost analysis
- [ ] Capacity planning

### Quarterly Tasks
- [ ] Major dependency updates
- [ ] Security penetration testing
- [ ] Disaster recovery drill
- [ ] Infrastructure review
- [ ] SSL certificate check
- [ ] Review and update runbooks

---

## Troubleshooting Guide

### Application Won't Start

#### Check Logs
```bash
# PM2 logs
pm2 logs backend --lines 100

# System logs
journalctl -u nginx -n 50
tail -f /var/log/syslog
```

#### Common Issues
1. **Port already in use**
   ```bash
   # Find process using port
   lsof -i :3000
   # Kill process
   kill -9 <PID>
   ```

2. **Environment variables missing**
   ```bash
   # Verify .env file exists
   cat /var/www/backend/.env
   ```

3. **Database connection failed**
   ```bash
   # Test database connection
   psql -U your_user -d your_database -h localhost
   ```

4. **Build errors**
   ```bash
   # Clear node_modules and rebuild
   rm -rf node_modules package-lock.json
   npm install
   npm run build
   ```

### High CPU Usage

#### Identify Process
```bash
# Top processes
top
htop

# PM2 process monitoring
pm2 monit
```

#### Solutions
- Scale horizontally (add more instances)
- Optimize slow code
- Add caching
- Increase server resources

### High Memory Usage

#### Check Memory
```bash
# Memory usage
free -h

# Process memory
ps aux --sort=-%mem | head

# PM2 memory
pm2 list
```

#### Solutions
- Identify memory leaks (profiling)
- Restart applications periodically
- Increase server RAM
- Optimize code

### Database Connection Issues

#### Check PostgreSQL
```bash
# Status
systemctl status postgresql

# Connection count
psql -U postgres -c "SELECT count(*) FROM pg_stat_activity;"

# Kill idle connections
psql -U postgres -c "SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE state = 'idle' AND state_change < current_timestamp - INTERVAL '5 minutes';"
```

#### Solutions
- Increase connection pool
- Use connection pooler (pgBouncer)
- Close connections properly
- Check for connection leaks

### Disk Space Full

#### Check Disk Usage
```bash
# Disk space
df -h

# Large directories
du -sh /* | sort -h

# Large files
find / -type f -size +100M
```

#### Clean Up
```bash
# Remove old logs
find /var/log -type f -name "*.log" -mtime +30 -delete

# Clean apt cache
apt clean

# Remove old Docker images
docker system prune -a

# Remove old backups
find /home/deploy/backups -mtime +30 -delete
```

### SSL Certificate Issues

#### Check Certificate
```bash
# Certificate expiration
openssl x509 -in /etc/letsencrypt/live/yourdomain.com/cert.pem -noout -dates

# Test SSL
openssl s_client -connect yourdomain.com:443
```

#### Renew Certificate
```bash
# Manual renewal
certbot renew

# Force renewal
certbot renew --force-renewal
```

### Nginx Issues

#### Test Configuration
```bash
# Test config syntax
nginx -t

# Reload without downtime
systemctl reload nginx

# Full restart
systemctl restart nginx
```

#### Common Nginx Errors
1. **502 Bad Gateway**: Backend not running
2. **504 Gateway Timeout**: Backend too slow
3. **413 Request Entity Too Large**: Increase `client_max_body_size`

---

## Quick Reference Commands

### PM2 Commands
```bash
pm2 start app                    # Start application
pm2 stop app                     # Stop application
pm2 restart app                  # Restart application
pm2 reload app                   # Reload without downtime
pm2 delete app                   # Remove from PM2
pm2 list                         # List applications
pm2 logs app                     # View logs
pm2 monit                        # Monitor resources
pm2 save                         # Save process list
pm2 startup                      # Setup auto-start
```

### Git Commands
```bash
git pull origin main             # Pull latest changes
git checkout -b feature/name     # Create new branch
git add .                        # Stage changes
git commit -m "message"          # Commit changes
git push origin branch           # Push to remote
git tag v1.0.0                   # Create tag
git push --tags                  # Push tags
```

### Docker Commands
```bash
docker-compose up -d             # Start containers
docker-compose down              # Stop containers
docker-compose logs -f           # View logs
docker-compose ps                # List containers
docker-compose restart service   # Restart service
docker-compose pull              # Pull latest images
docker exec -it container sh     # Shell into container
docker system prune -a           # Clean up
```

### Database Commands
```bash
psql -U postgres                 # Connect to PostgreSQL
\l                               # List databases
\c database                      # Connect to database
\dt                              # List tables
\q                               # Quit
pg_dump db > backup.sql          # Backup database
psql db < backup.sql             # Restore database
```

### Server Commands
```bash
systemctl start service          # Start service
systemctl stop service           # Stop service
systemctl restart service        # Restart service
systemctl status service         # Check status
systemctl enable service         # Enable on boot
journalctl -u service            # View logs
df -h                           # Disk usage
free -h                         # Memory usage
htop                            # Process monitor
```

---

## Additional Resources

### Documentation
- NestJS: https://docs.nestjs.com
- Angular: https://angular.io/docs
- Docker: https://docs.docker.com
- Nginx: https://nginx.org/en/docs
- PostgreSQL: https://www.postgresql.org/docs

### Learning Resources
- DigitalOcean Tutorials: https://www.digitalocean.com/community/tutorials
- AWS Documentation: https://docs.aws.amazon.com
- DevOps Roadmap: https://roadmap.sh/devops
- System Design Primer: https://github.com/donnemartin/system-design-primer

### Tools
- SSL Test: https://www.ssllabs.com/ssltest
- GTmetrix: https://gtmetrix.com
- Pingdom: https://www.pingdom.com
- StatusPage: https://www.atlassian.com/software/statuspage

---

**Document Version**: 1.0  
**Last Updated**: October 2025  
**Technologies**: NestJS, Angular, PostgreSQL, Docker, Nginx, PM2  
**Target**: Production-ready full-stack deployment
