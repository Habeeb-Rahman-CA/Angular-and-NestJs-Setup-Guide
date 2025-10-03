# Angular Frontend Development Environment Setup Guide
**Complete Production-Ready Configuration**

---

## Table of Contents
1. [Prerequisites Installation](#prerequisites-installation)
2. [Project Initialization](#project-initialization)
3. [Essential Packages Installation](#essential-packages-installation)
4. [Project Structure Setup](#project-structure-setup)
5. [Environment Configuration](#environment-configuration)
6. [Routing Configuration](#routing-configuration)
7. [State Management Setup](#state-management-setup)
8. [HTTP Client & Interceptors](#http-client--interceptors)
9. [Authentication & Authorization](#authentication--authorization)
10. [UI Component Library](#ui-component-library)
11. [Forms & Validation](#forms--validation)
12. [Error Handling & Logging](#error-handling--logging)
13. [Performance Optimization](#performance-optimization)
14. [Testing Setup](#testing-setup)
15. [Build & Deployment](#build--deployment)
16. [Production Checklist](#production-checklist)

---

## 1. Prerequisites Installation

### System Requirements

#### Node.js & npm
- **Node.js**: Install LTS version (v18 or v20)
  - Download from nodejs.org
  - Verify: `node --version` and `npm --version`
  - npm should be version 9.x or higher

#### Angular CLI
- **Install globally**:
```bash
npm install -g @angular/cli
```
- Verify: `ng version`
- Current stable: Angular 17.x or 18.x

#### Git
- For version control
- Verify: `git --version`
- Configure user name and email

#### Code Editor: VS Code (Recommended)
Essential extensions:
- **Angular Language Service** - IntelliSense for Angular
- **Angular Snippets** - Code shortcuts
- **ESLint** - Code linting
- **Prettier** - Code formatting
- **Auto Rename Tag** - Rename paired HTML tags
- **Path Intellisense** - Autocomplete file paths
- **Angular Schematics** - Generate components easily
- **GitLens** - Git integration
- **Bracket Pair Colorizer** - Visual bracket matching
- **Material Icon Theme** - Better file icons
- **Todo Tree** - Track TODO comments
- **Thunder Client** - API testing in VS Code

#### Browser Extensions
- **Angular DevTools** (Chrome/Edge)
  - Component inspection
  - Performance profiling
  - Change detection visualization
- **Redux DevTools** (if using NgRx)
- **JSON Viewer** - Format JSON responses

#### Additional Tools
- **Postman or Insomnia** - API testing
- **Chrome/Edge** - Latest version for development
- **Git GUI** (optional) - SourceTree, GitKraken, or GitHub Desktop

### Optional but Recommended
- **NVM (Node Version Manager)** - Manage multiple Node versions
- **Docker** - For containerization
- **Nx Console** (VS Code extension) - If using Nx workspace

---

## 2. Project Initialization

### Create New Angular Project
```bash
ng new project-name
```

### CLI Configuration Prompts

#### Routing
- **Question**: "Would you like to add Angular routing?"
- **Answer**: Yes (Y)
- **Why**: Essential for multi-page applications

#### Stylesheet Format
- **Question**: "Which stylesheet format would you like to use?"
- **Options**:
  - CSS (standard)
  - SCSS/SASS (recommended - more features)
  - Less
  - Stylus
- **Recommendation**: SCSS
- **Why**: Variables, nesting, mixins, better organization

#### Standalone Components
- Angular 17+ uses standalone components by default
- Simpler, less boilerplate
- No need for NgModules for every component

### Project Creation Process
- CLI will install dependencies
- Creates project structure
- Configures TypeScript
- Sets up development server
- Takes 2-5 minutes

### Navigate to Project
```bash
cd project-name
```

### Test Installation
```bash
ng serve
```
- Opens on `http://localhost:4200`
- Hot reload enabled
- Verify app loads successfully

### Initialize Git
```bash
git init
git add .
git commit -m "Initial Angular project setup"
```

### Connect to Remote Repository
- Create repository on GitHub/GitLab
- Add remote and push

### Install Latest Angular Version
If starting with older version:
```bash
ng update @angular/cli @angular/core
```

---

## 3. Essential Packages Installation

### UI Component Libraries

#### Option 1: Angular Material (Recommended)
```bash
ng add @angular/material
```
**Prompts during installation**:
- Choose theme (Indigo/Pink, Purple/Green, etc.)
- Set up typography styles: Yes
- Include animations: Yes

**Includes**:
- Material Design components
- CDK (Component Dev Kit)
- Pre-built themes
- Accessibility features

#### Option 2: PrimeNG (Feature-rich alternative)
```bash
npm install primeng primeicons
```
**Additional setup needed**:
- Import PrimeNG CSS
- Configure theme
- Rich component set

#### Option 3: Ant Design (NG-ZORRO)
```bash
ng add ng-zorro-antd
```
**Enterprise-grade UI**:
- Comprehensive components
- Internationalization
- High-quality design

#### Option 4: Bootstrap
```bash
npm install bootstrap @ng-bootstrap/ng-bootstrap
```
**Traditional choice**:
- Familiar framework
- Responsive grid
- Many components

**Recommendation**: Angular Material for consistency with Angular ecosystem

---

### State Management

#### NgRx (Redux pattern - Recommended for large apps)
```bash
ng add @ngrx/store
ng add @ngrx/effects
ng add @ngrx/entity
ng add @ngrx/store-devtools
ng add @ngrx/router-store
```
**When to use**:
- Large applications
- Complex state
- Many components sharing data
- Predictable state updates

#### Akita (Simpler alternative)
```bash
npm install @datorama/akita
npm install -D @datorama/akita-ngdevtools
```
**When to use**:
- Medium-sized apps
- Less boilerplate than NgRx
- Easier learning curve

#### NgRx Component Store (Local state)
```bash
npm install @ngrx/component-store
```
**When to use**:
- Component-level state
- Local feature state
- Simpler than full NgRx

#### RxJS (Already included)
- Reactive programming
- Observables
- Operators for data transformation

---

### HTTP & API Communication

#### HTTP Client (Built-in)
Already included in Angular

#### Axios (Alternative HTTP client)
```bash
npm install axios
```
**Optional**: Some developers prefer it

#### RxJS Operators Enhancement
```bash
npm install rxjs
```
Already included but ensure latest version

---

### Form Libraries

#### Angular Reactive Forms (Built-in)
Included in Angular

#### Additional Form Tools
```bash
npm install @angular/forms
```
Already included but useful to verify

#### Form Validation Enhancement
```bash
npm install ngx-custom-validators
```
**Features**:
- Email validation
- URL validation
- Phone number validation
- Credit card validation

---

### Authentication & Security

#### Angular JWT
```bash
npm install @auth0/angular-jwt
```
**Features**:
- JWT token management
- Automatic token attachment
- Token expiration handling

#### Auth0 (If using Auth0 service)
```bash
npm install @auth0/auth0-angular
```

#### OAuth/Social Login
```bash
npm install angularx-social-login
```
**Supports**:
- Google
- Facebook
- LinkedIn
- Microsoft

---

### Routing & Navigation

#### Angular Router (Built-in)
Already included

#### Router Guards Enhancement
Built-in but you'll create custom guards

#### Breadcrumbs
```bash
npm install xng-breadcrumb
```
**Features**:
- Automatic breadcrumb generation
- Customizable
- SEO-friendly

---

### Date & Time

#### Moment.js (Popular but heavy)
```bash
npm install moment
npm install -D @types/moment
```

#### Date-fns (Recommended - lighter)
```bash
npm install date-fns
```
**Why better**:
- Modular
- Smaller bundle size
- Immutable

#### Day.js (Lightest alternative)
```bash
npm install dayjs
```

#### Angular Date Pipe (Built-in)
Use built-in for simple cases

---

### Charts & Visualization

#### Chart.js with Angular
```bash
npm install chart.js ng2-charts
```
**Chart types**:
- Line, Bar, Pie, Doughnut
- Radar, Polar, Bubble
- Responsive

#### ApexCharts
```bash
npm install apexcharts ng-apexcharts
```
**Modern alternative**:
- Beautiful designs
- Interactive
- Many chart types

#### D3.js (Advanced)
```bash
npm install d3
npm install -D @types/d3
```
**For custom visualizations**

---

### File Upload

#### ng2-file-upload
```bash
npm install ng2-file-upload
npm install -D @types/ng2-file-upload
```
**Features**:
- Multiple files
- Drag and drop
- Progress tracking

#### ngx-dropzone
```bash
npm install ngx-dropzone
```
**Modern alternative**

---

### Notifications & Toasts

#### ngx-toastr
```bash
npm install ngx-toastr
```
**Features**:
- Success, error, warning, info messages
- Customizable position
- Auto-dismiss
- Progress bar

#### Angular Material Snackbar
Built-in with Material

---

### Loading Indicators

#### ngx-spinner
```bash
npm install ngx-spinner
```
**Features**:
- Multiple spinner types
- Full-screen overlay
- Custom templates

#### ngx-loading
```bash
npm install ngx-loading
```

---

### Utilities

#### Lodash
```bash
npm install lodash
npm install -D @types/lodash
```
**Utility functions**:
- Array manipulation
- Object operations
- Debounce, throttle
- Deep clone

#### UUID Generation
```bash
npm install uuid
npm install -D @types/uuid
```

#### RxJS Extensions
```bash
npm install rxjs-compat
```
Only if migrating older code

---

### Internationalization (i18n)

#### ngx-translate
```bash
npm install @ngx-translate/core @ngx-translate/http-loader
```
**Features**:
- Multiple languages
- Dynamic translation
- Lazy loading translations

#### Angular i18n (Built-in)
Alternative: Use Angular's built-in i18n

---

### Icons

#### Font Awesome
```bash
npm install @fortawesome/fontawesome-free
```

#### Material Icons
Included with Angular Material

#### Feather Icons
```bash
npm install angular-feather
npm install feather-icons
```

---

### Pagination

#### ngx-pagination
```bash
npm install ngx-pagination
```
**Features**:
- Customizable pagination
- Multiple instances
- Server-side pagination support

---

### Tables

#### Angular Material Table
Included with Material

#### ngx-datatable
```bash
npm install @swimlane/ngx-datatable
```
**Advanced features**:
- Virtual scrolling
- Row grouping
- Tree table
- Cell templates

---

### PDF Generation

#### jsPDF
```bash
npm install jspdf
npm install -D @types/jspdf
```

#### html2canvas (For PDF from HTML)
```bash
npm install html2canvas
npm install -D @types/html2canvas
```

---

### Excel Export

#### SheetJS (xlsx)
```bash
npm install xlsx
npm install -D @types/xlsx
```
**Features**:
- Read/Write Excel files
- Multiple formats
- Data export

---

### Rich Text Editor

#### ngx-quill
```bash
npm install ngx-quill quill
```
**Features**:
- WYSIWYG editor
- Customizable toolbar
- Image upload

#### TinyMCE
```bash
npm install @tinymce/tinymce-angular
```

---

### Development Dependencies

#### TypeScript Types
```bash
npm install -D @types/node
```

#### ESLint (Modern linting)
```bash
ng add @angular-eslint/schematics
```
**Replaces TSLint** (deprecated)

#### Prettier (Code formatting)
```bash
npm install -D prettier eslint-config-prettier eslint-plugin-prettier
```

#### Husky (Git hooks)
```bash
npm install -D husky
npx husky init
```
**Pre-commit checks**:
- Linting
- Tests
- Formatting

#### Lint-staged
```bash
npm install -D lint-staged
```
**Lint only staged files**

#### Compodoc (Documentation)
```bash
npm install -D @compodoc/compodoc
```
**Generate documentation** from code comments

---

### Testing Enhancement

#### Karma & Jasmine (Included)
Default testing framework

#### Jest (Alternative - Faster)
```bash
npm install -D jest @types/jest jest-preset-angular
```

#### Cypress (E2E Testing)
```bash
npm install -D cypress
```
**Better than Protractor**

#### Testing Library
```bash
npm install -D @testing-library/angular
```
**Better testing practices**

---

### Performance Monitoring

#### Web Vitals
```bash
npm install web-vitals
```
**Measure**:
- LCP, FID, CLS
- Performance metrics

---

### Error Tracking

#### Sentry
```bash
npm install @sentry/angular
```
**Production error tracking**

---

### PWA (Progressive Web App)

#### Angular PWA
```bash
ng add @angular/pwa
```
**Features**:
- Service worker
- Offline capability
- App manifest
- Installable

---

### Security

#### DOMPurify (XSS Protection)
```bash
npm install dompurify
npm install -D @types/dompurify
```
**Sanitize HTML content**

---

## 4. Project Structure Setup

### Recommended Folder Structure

```
src/
├── app/
│   ├── core/
│   │   ├── guards/
│   │   │   ├── auth.guard.ts
│   │   │   └── role.guard.ts
│   │   ├── interceptors/
│   │   │   ├── auth.interceptor.ts
│   │   │   ├── error.interceptor.ts
│   │   │   └── loading.interceptor.ts
│   │   ├── services/
│   │   │   ├── auth.service.ts
│   │   │   ├── api.service.ts
│   │   │   ├── storage.service.ts
│   │   │   └── logger.service.ts
│   │   ├── models/
│   │   │   ├── user.model.ts
│   │   │   └── response.model.ts
│   │   ├── constants/
│   │   │   ├── api-endpoints.ts
│   │   │   └── app-constants.ts
│   │   └── core.module.ts
│   ├── shared/
│   │   ├── components/
│   │   │   ├── header/
│   │   │   ├── footer/
│   │   │   ├── sidebar/
│   │   │   ├── loader/
│   │   │   └── notification/
│   │   ├── directives/
│   │   │   ├── highlight.directive.ts
│   │   │   └── tooltip.directive.ts
│   │   ├── pipes/
│   │   │   ├── truncate.pipe.ts
│   │   │   ├── safe-html.pipe.ts
│   │   │   └── format-date.pipe.ts
│   │   ├── validators/
│   │   │   └── custom-validators.ts
│   │   └── shared.module.ts
│   ├── features/
│   │   ├── auth/
│   │   │   ├── login/
│   │   │   ├── register/
│   │   │   ├── forgot-password/
│   │   │   └── auth-routing.module.ts
│   │   ├── dashboard/
│   │   │   ├── dashboard.component.ts
│   │   │   └── dashboard-routing.module.ts
│   │   ├── users/
│   │   │   ├── user-list/
│   │   │   ├── user-detail/
│   │   │   ├── user-form/
│   │   │   └── users-routing.module.ts
│   │   └── [other-features]/
│   ├── layout/
│   │   ├── main-layout/
│   │   ├── auth-layout/
│   │   └── admin-layout/
│   ├── store/ (if using NgRx)
│   │   ├── actions/
│   │   ├── reducers/
│   │   ├── effects/
│   │   ├── selectors/
│   │   └── state/
│   ├── app.component.ts
│   ├── app.component.html
│   ├── app.component.scss
│   ├── app.config.ts
│   └── app.routes.ts
├── assets/
│   ├── images/
│   ├── icons/
│   ├── fonts/
│   ├── i18n/ (translation files)
│   │   ├── en.json
│   │   └── es.json
│   └── styles/
│       ├── variables.scss
│       ├── mixins.scss
│       └── themes/
├── environments/
│   ├── environment.ts
│   ├── environment.development.ts
│   ├── environment.staging.ts
│   └── environment.production.ts
├── styles.scss (global styles)
├── index.html
└── main.ts
```

### Create Folder Structure
```bash
# Core module structure
ng g module core
ng g service core/services/auth
ng g service core/services/api
ng g service core/services/storage
ng g guard core/guards/auth
ng g interceptor core/interceptors/auth

# Shared module structure
ng g module shared
ng g component shared/components/header
ng g component shared/components/footer
ng g pipe shared/pipes/truncate

# Feature modules
ng g module features/auth --routing
ng g component features/auth/login
ng g component features/auth/register

# Layout components
ng g component layout/main-layout
ng g component layout/auth-layout
```

### Module Organization Strategy

#### Core Module (Singleton Services)
- Import once in AppModule
- Services used throughout app
- Guards and interceptors
- HTTP services
- Authentication services
- Never import in feature modules

#### Shared Module
- Import in feature modules
- Reusable components
- Directives and pipes
- Common UI elements
- No services (use Core for services)

#### Feature Modules
- Lazy loaded when possible
- Self-contained features
- Own routing
- Feature-specific components
- Import SharedModule

---

## 5. Environment Configuration

### Environment Files Structure

#### environment.ts (Development - Default)
```typescript
// This is used during ng serve
```
**Contains**:
- API base URL (localhost)
- Feature flags
- Debug mode enabled
- Console logging level
- Mock API endpoints (if using)

#### environment.development.ts
**Explicit development config**:
- Development API endpoints
- Debug tokens
- Relaxed security
- Verbose logging

#### environment.staging.ts
**Staging environment**:
- Staging API URLs
- Test payment gateways
- Moderate logging
- Similar to production

#### environment.production.ts
**Production configuration**:
- Production API URLs
- Error logging only
- Analytics enabled
- No debug info
- Strict security
- CDN URLs
- API keys (loaded securely)

### Environment Variable Best Practices

#### What to Include
- API base URLs
- API versions
- Feature toggles
- Environment name
- Logging levels
- Third-party API keys (public ones)
- OAuth client IDs
- Analytics IDs
- Socket URLs
- CDN URLs

#### What NOT to Include
- Secret keys
- Private API keys
- Database credentials
- JWT secrets

### Configuration Service Pattern
Create a ConfigService to centralize environment access:
- Type-safe configuration
- Runtime configuration loading
- Environment detection
- Feature flag evaluation

### Build Configuration

#### angular.json Configurations
Define build configurations:
- Development
- Staging  
- Production
- Custom configurations

#### File Replacements
Configure which environment file to use per configuration

#### Build Commands
```bash
ng build                           # Development
ng build --configuration staging   # Staging
ng build --configuration production # Production
```

---

## 6. Routing Configuration

### App Routes Setup (app.routes.ts)

#### Basic Routing Structure
- Root routes
- Redirect strategies
- 404 page
- Wildcard routes

#### Route Configuration Options
- path: URL segment
- component: Component to display
- loadChildren: Lazy loaded module
- canActivate: Guards array
- data: Extra data passed to route
- title: Page title (Angular 14+)

### Lazy Loading Strategy

#### Why Lazy Load
- Faster initial load
- Smaller bundle size
- Better performance
- Load on demand

#### Feature Module Lazy Loading
Configure loadChildren for feature routes

#### Preloading Strategies
- PreloadAllModules (load in background)
- Custom preloading
- No preloading (default)

### Route Guards

#### Auth Guard
- Check if user is authenticated
- Redirect to login if not
- Protect private routes

#### Role Guard
- Check user permissions
- Role-based access control
- Admin vs User routes

#### Unsaved Changes Guard
- Prevent navigation with unsaved data
- Confirm dialog
- Can Deactivate interface

### Router Outlet Strategies

#### Named Outlets
Multiple router outlets on same page

#### Auxiliary Routes
Side panels, modals, etc.

### Navigation Guards Order
- canActivate
- canActivateChild
- canLoad
- canDeactivate

### Router Events

#### Navigation Events
- NavigationStart
- NavigationEnd
- NavigationError
- NavigationCancel

#### Use Cases
- Loading indicators
- Analytics tracking
- Error logging
- Page title updates

### Route Resolvers

#### Pre-fetch Data
Load data before route activation
- Resolve interface
- Return Observable/Promise
- Better UX (no loading state in component)

### Router Configuration Best Practices
- Use absolute paths
- Consistent naming conventions
- Group related routes
- Use path parameters wisely
- Implement 404 page
- Handle navigation errors

---

## 7. State Management Setup

### NgRx Setup (For Large Applications)

#### Core Concepts
- **Store**: Single source of truth
- **Actions**: Events in app
- **Reducers**: Pure functions that update state
- **Effects**: Side effects (API calls)
- **Selectors**: Query state

#### Installation Steps
Already covered in packages section

#### Store Structure
```
store/
├── actions/
│   ├── user.actions.ts
│   └── product.actions.ts
├── reducers/
│   ├── user.reducer.ts
│   ├── product.reducer.ts
│   └── index.ts (root reducer)
├── effects/
│   ├── user.effects.ts
│   └── product.effects.ts
├── selectors/
│   ├── user.selectors.ts
│   └── product.selectors.ts
└── state/
    ├── user.state.ts
    ├── product.state.ts
    └── app.state.ts
```

#### Store Configuration
- Register in AppConfig
- Import StoreModule.forRoot()
- Register effects
- Setup dev tools (development only)
- Router store integration

#### Action Patterns
- Use action creators
- Descriptive action names
- Type safety with createAction
- Action payload interfaces

#### Reducer Best Practices
- Pure functions only
- No mutations (use spread operator)
- Handle all action types
- Use createReducer for simplicity
- Default state definition

#### Effects Guidelines
- Handle async operations
- API calls
- Navigate after actions
- Error handling
- Use createEffect()

#### Selectors Usage
- Memoized state queries
- Combine multiple selectors
- Computed values
- Performance optimization

### Component Store (Local State)

#### When to Use
- Component-specific state
- No need for global state
- Simple state management
- Feature-level state

#### Setup
- Extend ComponentStore
- Define state interface
- Create updaters
- Create selectors
- Create effects

### Services as State (Simple Apps)

#### BehaviorSubject Pattern
- For small applications
- No external library needed
- Simple state container
- Observable-based

#### Service Structure
- Private BehaviorSubject
- Public Observable
- Update methods
- Getter methods

### State Management Decision Tree
- **Small app**: Services with BehaviorSubject
- **Medium app**: NgRx Component Store
- **Large app**: Full NgRx store
- **Complex forms**: Reactive Forms state

---

## 8. HTTP Client & Interceptors

### HTTP Client Setup

#### Import HttpClient
Already included but needs to be provided

#### Configuration
- Import provideHttpClient in app.config
- Enable interceptors
- Configure timeout
- Set base URL

### HTTP Service Pattern

#### Create Base API Service
Centralized HTTP operations:
- GET, POST, PUT, DELETE methods
- Error handling
- Response transformation
- Type safety with generics

#### Feature-Specific Services
Extend base API service:
- User service
- Product service
- Auth service
- Each handles own endpoints

### HTTP Interceptors

#### Auth Interceptor
**Purpose**: Add JWT token to requests
- Read token from storage
- Attach to Authorization header
- Skip for public endpoints
- Handle token expiration

#### Error Interceptor
**Purpose**: Global error handling
- Catch HTTP errors
- 401: Redirect to login
- 403: Show permission error
- 500: Show server error
- Network errors
- Log errors

#### Loading Interceptor
**Purpose**: Show loading indicator
- Start loader on request
- Stop loader on response
- Count simultaneous requests
- Debounce for UX

#### Retry Interceptor
**Purpose**: Retry failed requests
- Retry network failures
- Exponential backoff
- Max retry attempts
- Skip for certain status codes

#### Cache Interceptor
**Purpose**: Cache GET requests
- Store responses
- Return cached data
- TTL (time to live)
- Cache invalidation

#### Logging Interceptor
**Purpose**: Log HTTP traffic (development)
- Request details
- Response details
- Duration
- Disable in production

### Interceptor Chain
Multiple interceptors execute in order:
1. Auth Interceptor
2. Loading Interceptor
3. Logging Interceptor
4. Error Interceptor
5. Cache Interceptor

### HTTP Client Configuration

#### Timeout Configuration
Set global timeout for requests

#### Base URL Configuration
Set API base URL from environment

#### HTTP Headers
Default headers for all requests:
- Content-Type
- Accept
- Custom headers

### Error Handling Strategy

#### HTTP Error Types
- Client errors (4xx)
- Server errors (5xx)
- Network errors
- Timeout errors

#### User-Friendly Messages
Map error codes to messages:
- 400: Validation error
- 401: Unauthorized
- 403: Forbidden
- 404: Not found
- 500: Server error

#### Error Notification
- Show toasts/snackbars
- Log to console (dev)
- Send to error tracking (prod)

### Request/Response Transformation

#### Request Transformation
- Format dates
- Transform property names
- Add computed properties

#### Response Transformation
- Parse dates
- Map to models
- Handle null/undefined

### HTTP Client Best Practices
- Type all responses
- Handle errors globally
- Use interceptors wisely
- Cancel requests when needed (takeUntil)
- Avoid nested subscriptions
- Use async pipe in templates

---

## 9. Authentication & Authorization

### Authentication Strategy

#### JWT Token-Based Auth (Most Common)
- User logs in
- Server returns JWT access token
- Store token securely
- Attach to requests via interceptor
- Refresh token mechanism

### Token Storage Options

#### LocalStorage
**Pros**: Persists across tabs
**Cons**: XSS vulnerable
**Use case**: Lower security requirements

#### SessionStorage
**Pros**: Cleared on tab close
**Cons**: Still XSS vulnerable
**Use case**: Temporary sessions

#### HttpOnly Cookies (Recommended)
**Pros**: XSS protection
**Cons**: Requires server setup
**Use case**: High security needs

#### Memory (Most Secure)
**Pros**: No XSS risk
**Cons**: Lost on refresh
**Use case**: With refresh token

### Auth Service Structure

#### Core Functions
- login(): Authenticate user
- logout(): Clear session
- register(): Create account
- refreshToken(): Get new access token
- isAuthenticated(): Check login status
- getCurrentUser(): Get user data
- hasRole(): Check permissions

#### Token Management
- Store tokens securely
- Auto-refresh before expiration
- Handle refresh failures
- Clear on logout

### Auth Guard Implementation

#### Basic Auth Guard
- Check if user is logged in
- Redirect to login if not
- Allow navigation if authenticated
- Store return URL for redirect after login

#### Role-Based Guard
- Check user roles/permissions
- Allow/deny based on role
- Show unauthorized page
- Multiple roles support

### Login Flow

#### Step-by-Step
1. User enters credentials
2. Submit to backend API
3. Receive JWT token
4. Store token securely
5. Redirect to dashboard
6. Set user state

#### Remember Me Feature
- Longer token expiration
- Store in localStorage
- Auto-login on return

### Logout Flow

#### Complete Cleanup
1. Clear tokens from storage
2. Clear user state
3. Cancel pending requests
4. Redirect to login
5. Optional: Notify backend

### Token Refresh Strategy

#### Silent Refresh
- Refresh before expiration
- Background process
- User doesn't notice
- Handle refresh failures

#### Refresh Token Flow
1. Access token expires
2. Use refresh token to get new access token
3. Store new tokens
4. Retry failed request
5. If refresh fails, logout

### Protected Routes

#### Route Configuration
- Apply auth guard to routes
- Protect entire modules
- Public vs private routes
- Different guards for different sections

### User State Management

#### Store User Data
- User profile
- Permissions/roles
- Preferences
- Use state management (NgRx or service)

#### Update User Data
- Profile updates
- Role changes
- Real-time updates

### Social Authentication

#### OAuth 2.0 Flow
- Google Sign-In
- Facebook Login
- GitHub Auth
- Microsoft Auth

#### Implementation
- Use social login library
- Configure OAuth apps
- Handle callbacks
- Merge with existing auth

### Multi-Factor Authentication (MFA)

#### Setup
- SMS codes
- Authenticator apps (TOTP)
- Email verification
- Backup codes

### Session Management

#### Session Timeout
- Auto-logout after inactivity
- Warn before timeout
- Save work before logout

#### Concurrent Sessions
- Detect multiple logins
- Force logout other sessions
- Session limits

### Security Best Practices

#### Authentication Security
- HTTPS only
- Secure token storage
- XSS protection
- CSRF protection
- Rate limiting login attempts
- Password policies
- Account lockout

#### Authorization Security
- Principle of least privilege
- Regular permission audits
- Role hierarchy
- Resource-level permissions

---

## 10. UI Component Library

### Angular Material Setup

#### Initial Configuration
Already added via `ng add @angular/material`

#### Theme Customization

##### Pre-built Themes
- Indigo-Pink
- Deep Purple-Amber
- Pink-Blue Grey
- Purple-Green

##### Custom Theme
Create custom theme in styles.scss:
- Define primary color palette
- Define accent color palette
- Define warn color palette
- Dark mode support
- Typography configuration

#### Component Imports

##### Material Modules Structure
Create material.module.ts to organize imports:
- MatButtonModule
- MatCardModule
- MatToolbarModule
- MatSidenavModule
- MatIconModule
- MatListModule
- MatTableModule
- MatPaginatorModule
- MatSortModule
- MatInputModule
- MatFormFieldModule
- MatSelectModule
- MatCheckboxModule
- MatRadioModule
- MatDatepickerModule
- MatDialogModule
- MatSnackBarModule
- MatProgressSpinnerModule
- MatProgressBarModule
- MatTooltipModule
- MatMenuModule
- MatBadgeModule
- MatChipsModule
- MatSlideToggleModule
- MatSliderModule
- MatTabsModule
- MatStepperModule
- MatExpansionModule
- MatAutocompleteModule

##### Import Strategy
- Create shared material module
- Import only needed components
- Export for use in other modules
- Reduces bundle size

#### CDK (Component Dev Kit)

##### Key CDK Modules
- **DragDropModule**: Drag and drop functionality
- **ScrollingModule**: Virtual scrolling
- **OverlayModule**: Overlay/popup system
- **A11yModule**: Accessibility helpers
- **PortalModule**: Dynamic component loading
- **TableModule**: Data table foundation

##### CDK Use Cases
- Custom components
- Advanced interactions
- Performance optimization
- Accessibility features

#### Material Icons Setup

##### Icon Configuration
- Import MatIconModule
- Register icon sets
- Use Material Icons font
- Custom SVG icons

##### Icon Usage
- mat-icon component
- Icon button
- Icon with text
- Custom icon registry

#### Responsive Design with Material

##### Breakpoint Observer
- Detect screen size
- Mobile vs desktop layouts
- Adaptive components
- Responsive navigation

##### Flex Layout (Deprecated)
Use CSS Grid/Flexbox instead:
- Modern CSS approaches
- Better performance
- Native browser support

### Alternative: PrimeNG

#### Theme Selection
PrimeNG offers many themes:
- Material Design themes
- Bootstrap themes
- Custom themes
- Dark mode variants

#### Component Coverage
150+ components:
- Data tables
- Charts
- Forms
- Overlays
- File upload
- Calendar
- Tree
- More feature-rich than Material

#### PrimeNG Configuration
- Import CSS files
- Import PrimeNG modules
- Configure theme
- Set up PrimeIcons

### Alternative: Ant Design (NG-ZORRO)

#### Enterprise-Grade Features
- Comprehensive component set
- Design system
- i18n support
- TypeScript support
- Regular updates

#### Configuration
- Add via ng add
- Import modules
- Configure theme
- Set locale

### UI Library Decision Guide

#### Choose Angular Material if:
- Want Material Design look
- Need tight Angular integration
- Building internal tools
- Want official Angular support
- Need strong accessibility

#### Choose PrimeNG if:
- Need extensive component library
- Want more features out-of-box
- Building data-heavy apps
- Need advanced data tables
- Want theme flexibility

#### Choose Ant Design if:
- Building enterprise apps
- Need comprehensive design system
- Want professional look
- Need Chinese language support
- Want active community

### Component Library Best Practices

#### Module Organization
- Don't import everything
- Tree-shakeable imports
- Lazy load heavy components
- Share common imports

#### Theming Strategy
- Consistent color palette
- Design tokens
- CSS variables
- Dark mode support

#### Customization
- Override component styles
- Custom themes
- Brand colors
- Maintain consistency

---

## 11. Forms & Validation

### Reactive Forms (Recommended)

#### Why Reactive Forms
- More control
- Type safety
- Easier testing
- Better for complex forms
- Synchronous access to data
- Easier to validate

#### Form Structure
- FormControl: Single field
- FormGroup: Group of controls
- FormArray: Dynamic list of controls
- FormBuilder: Helper service

#### Basic Form Setup
- Import ReactiveFormsModule
- Inject FormBuilder
- Create FormGroup
- Bind to template
- Handle submission

### Template-Driven Forms

#### When to Use
- Simple forms
- Quick prototypes
- Less TypeScript code
- More HTML-centric

#### Setup
- Import FormsModule
- Use ngModel directive
- Template reference variables
- Validation in template

### Form Validation

#### Built-in Validators
- required
- minLength
- maxLength
- min (number)
- max (number)
- email
- pattern (regex)

#### Custom Validators

##### Synchronous Validators
- Validate immediately
- Return validation error object
- Null if valid
- Examples: password match, unique username

##### Asynchronous Validators
- Async validation (API calls)
- Check username availability
- Email verification
- Return Observable or Promise

#### Validation Messages

##### Display Strategy
- Show on touched and invalid
- Show on submitted
- Real-time validation
- Inline vs summary

##### User-Friendly Messages
Map validation errors to readable text:
- required: "This field is required"
- email: "Please enter valid email"
- minLength: "Minimum X characters required"
- Custom error messages

### Form State Management

#### Form States
- pristine/dirty: Has value changed
- touched/untouched: Has field been focused
- valid/invalid: Validation status
- pending: Async validation in progress

#### State-Based UI
- Disable submit until valid
- Show validation only after touch
- Visual feedback (colors, icons)
- Loading state during async validation

### Dynamic Forms

#### Form Arrays
- Add/remove form controls dynamically
- Lists of items
- Repeating form sections
- Dynamic validation

#### Conditional Fields
- Show/hide based on values
- Enable/disable controls
- Dynamic validation rules
- Dependent fields

### Form Submission

#### Submit Handling
- Prevent default submission
- Validate before submit
- Show loading state
- Handle success/error
- Reset or redirect

#### Error Handling
- Backend validation errors
- Display field-specific errors
- General error messages
- Retry mechanism

### Advanced Form Patterns

#### Multi-Step Forms (Wizards)
- Angular Material Stepper
- Progress indicator
- Save partial data
- Validation per step
- Navigation between steps

#### Form Builder Pattern
- Reusable form configurations
- Dynamic form generation
- JSON-driven forms
- Form metadata

#### Autosave
- Save on value changes
- Debounce input
- Background save
- Show save status

### Form Accessibility

#### A11y Best Practices
- Label all inputs
- ARIA attributes
- Error announcements
- Keyboard navigation
- Focus management

### Form Performance

#### Optimization Techniques
- OnPush change detection
- Debounce input validation
- Lazy validation
- Virtual scrolling for long lists
- Unsubscribe from observables

### Form Security

#### Input Sanitization
- Validate on frontend AND backend
- Sanitize HTML inputs
- Prevent XSS
- Escape special characters

#### CSRF Protection
- Use tokens for state-changing operations
- HttpOnly cookies
- SameSite attribute

---

## 12. Error Handling & Logging

### Global Error Handler

#### Custom Error Handler
- Extend Angular's ErrorHandler
- Catch all unhandled errors
- Log to console/service
- Show user-friendly message
- Don't expose sensitive info

#### Error Handler Setup
- Create error handler service
- Provide in app config
- Override default handler

### HTTP Error Handling

#### Interceptor Approach
Already covered in HTTP section
- Centralized error handling
- Consistent error responses
- User notifications

#### Error Types to Handle
- Network errors (no connection)
- Timeout errors
- Server errors (5xx)
- Client errors (4xx)
- Parsing errors

### Error Notification System

#### User Notifications
- Toast/Snackbar for errors
- Error severity levels
- Auto-dismiss for minor errors
- Manual dismiss for critical errors
- Action buttons (retry, dismiss)

#### Error Pages
- 404 Not Found page
- 403 Forbidden page
- 500 Server Error page
- Network Error page
- Custom error layouts

### Logging Strategy

#### Log Levels
- **ERROR**: Critical issues
- **WARN**: Warning messages
- **INFO**: General information
- **DEBUG**: Detailed debug info
- **TRACE**: Very detailed logs

#### Logger Service

##### Features
- Configurable log levels
- Environment-based logging
- Console logging (development)
- Remote logging (production)
- Structured logging
- Context information

##### What to Log
- Errors and exceptions
- API requests/responses (dev)
- User actions
- Performance metrics
- State changes
- Navigation events

##### What NOT to Log
- Passwords
- API keys
- Personal data (PII)
- Credit card numbers
- Session tokens

### Remote Error Tracking

#### Sentry Integration
- Real-time error tracking
- Stack traces
- User context
- Breadcrumbs
- Release tracking
- Source maps

#### Setup Steps
- Install @sentry/angular
- Initialize in main.ts
- Configure DSN
- Set environment
- Enable source maps
- Test error tracking

#### Error Context
Attach useful information:
- User ID
- User agent
- Current route
- Application version
- Environment
- Custom tags

### Application Monitoring

#### Performance Monitoring
- Page load times
- API response times
- Component render times
- Bundle size monitoring
- Memory usage

#### User Monitoring
- User flows
- Feature usage
- Error rates
- Crash reports

### Error Recovery Strategies

#### Retry Logic
- Automatic retry for network errors
- Exponential backoff
- Max retry attempts
- User-initiated retry

#### Fallback Mechanisms
- Cached data when offline
- Default values
- Graceful degradation
- Alternative UI

#### Error Boundaries
- Isolate component errors
- Prevent full app crash
- Show error UI
- Reload component option

### Development vs Production

#### Development Logging
- Verbose logging
- Console output
- Detailed error messages
- Stack traces
- Debug information

#### Production Logging
- Error level only
- Remote logging service
- Generic user messages
- No sensitive data
- Performance optimized

### Logging Best Practices
- Use structured logging (JSON)
- Include timestamps
- Add correlation IDs
- Log context, not just errors
- Regular log review
- Set up alerts for critical errors
- Rotate logs
- Comply with privacy laws

---

## 13. Performance Optimization

### Build Optimization

#### Production Build
```bash
ng build --configuration production
```
**Optimizations applied**:
- AOT (Ahead-of-Time) compilation
- Tree shaking
- Minification
- Uglification
- Dead code elimination

#### Bundle Size Optimization

##### Lazy Loading
- Load routes on demand
- Reduce initial bundle
- Faster first load
- Better code splitting

##### Code Splitting
- Separate vendor bundles
- Common chunks
- Dynamic imports
- Route-based splitting

##### Bundle Analysis
```bash
npm install -D webpack-bundle-analyzer
ng build --stats-json
```
- Visualize bundle size
- Identify large dependencies
- Find optimization opportunities

##### Tree Shaking
- Remove unused code
- Import only what's needed
- Use ES6 modules
- Avoid side effects

#### Source Maps
- Enable for debugging production
- Upload to error tracking
- Don't serve to public
- Use hidden source maps

### Runtime Optimization

#### Change Detection Strategy

##### Default Change Detection
- Checks entire component tree
- Can be slow for large apps
- Simple but inefficient

##### OnPush Strategy
- Only check when inputs change
- Manual change detection
- Much better performance
- Use for smart components
- Requires immutable data

##### Detach Change Detection
- Manual control
- For rarely-changing components
- Complex optimization

#### Lazy Loading Images

##### Native Lazy Loading
```html
<img loading="lazy">
```
- Browser native support
- Easy implementation
- Good performance

##### Custom Lazy Loading
- ngx-lazy-load-images
- Intersection Observer
- Placeholder images
- Progressive loading

#### Virtual Scrolling

##### CDK Virtual Scroll
- Render only visible items
- Handle thousands of items
- Smooth scrolling
- Reduced DOM nodes

##### Use Cases
- Long lists
- Large tables
- Infinite scroll
- Chat messages

### Memory Management

#### Unsubscribe from Observables

##### Strategies
- takeUntil operator with destroy subject
- async pipe (auto-unsubscribes)
- take(1) for single emission
- First operator
- Manual unsubscribe

##### Memory Leak Prevention
- Unsubscribe in ngOnDestroy
- Clear timers/intervals
- Remove event listeners
- Clear references

#### Memory Profiling
- Chrome DevTools Memory tab
- Heap snapshots
- Allocation timeline
- Find memory leaks

### Rendering Performance

#### Track By Function
For *ngFor loops:
- Improve list rendering
- Reuse DOM elements
- Reduce re-rendering
- Identity tracking

#### Avoid Expensive Operations
- Move calculations to services
- Cache computed values
- Memoization
- Debounce/throttle

#### Pure Pipes
- Use pure pipes (default)
- Cached results
- Only recalculate when inputs change
- Better than functions in templates

### Network Optimization

#### HTTP Caching
- Cache GET requests
- ETags
- Cache-Control headers
- Stale-while-revalidate

#### Request Batching
- Combine multiple requests
- GraphQL for flexible queries
- Reduce HTTP overhead

#### Compression
- Enable gzip/brotli
- Server-side compression
- Smaller payload sizes

#### CDN Usage
- Serve static assets from CDN
- Closer to users
- Better performance
- Reduced server load

### Image Optimization

#### Responsive Images
- srcset attribute
- Multiple sizes
- Picture element
- Art direction

#### Image Formats
- WebP for modern browsers
- AVIF for best compression
- Fallback to JPEG/PNG
- Lazy loading

#### Image Compression
- Optimize before upload
- Use build-time optimization
- On-demand processing
- Progressive JPEGs

### Loading Strategy

#### Progressive Loading
- Show skeleton screens
- Load critical content first
- Stream data
- Defer non-critical resources

#### Preloading
- Preload next likely routes
- Prefetch data
- Preconnect to domains
- DNS prefetch

### Service Worker & PWA

#### Caching Strategies
- Cache-first
- Network-first
- Stale-while-revalidate
- Network-only
- Cache-only

#### Offline Support
- Cache app shell
- Cache API responses
- Offline page
- Background sync

### Angular-Specific Optimizations

#### Standalone Components
- Less boilerplate
- Better tree shaking
- Smaller bundles
- Faster compilation

#### Signals (Angular 16+)
- New reactivity system
- Better performance
- Fine-grained updates
- Simpler than RxJS for some cases

#### Defer Loading (Angular 17+)
```html
@defer
```
- Lazy load component templates
- On viewport, interaction, or timer
- Reduces initial bundle

### Performance Monitoring

#### Core Web Vitals
- LCP (Largest Contentful Paint)
- FID (First Input Delay)
- CLS (Cumulative Layout Shift)

#### Angular Performance Tools
- Angular DevTools profiler
- Change detection profiler
- Component tree analysis

#### Third-Party Tools
- Lighthouse
- WebPageTest
- Chrome DevTools Performance tab
- Real User Monitoring (RUM)

### Performance Budget

#### Set Limits
- Initial bundle size < 170KB
- Total bundle size < 500KB
- First Contentful Paint < 1.8s
- Time to Interactive < 3.9s

#### Enforce Budget
Configure in angular.json:
- Warning thresholds
- Error thresholds
- Fail build if exceeded

---

## 14. Testing Setup

### Testing Framework

#### Karma & Jasmine (Default)
Pre-configured with Angular:
- Unit testing
- Component testing
- Service testing
- Browser-based

#### Jest (Alternative - Recommended)
Faster and more modern:
- Parallel test execution
- Better mocking
- Snapshot testing
- Watch mode
- Coverage reports

##### Migration to Jest
- Install jest packages
- Remove Karma/Jasmine
- Update test configuration
- Update test scripts

### Unit Testing

#### Component Testing

##### Test Structure
- Setup TestBed
- Create component fixture
- Test component properties
- Test methods
- Test template bindings
- Test outputs/events

##### Best Practices
- Test behavior, not implementation
- Mock dependencies
- Test edge cases
- Test error scenarios
- Keep tests simple
- One assertion per test (ideally)

#### Service Testing

##### Isolated Tests
- No TestBed needed for simple services
- Just instantiate and test
- Mock dependencies
- Test all methods
- Test error handling

##### With Dependencies
- Use TestBed for DI
- Provide mock services
- Test integration

#### Pipe Testing
- Simple to test
- Pure functions
- Test transform method
- Test various inputs

#### Directive Testing
- Create host component
- Test DOM manipulation
- Test input/output
- Test behavior

### Integration Testing

#### Testing with Dependencies
- Real services (not mocked)
- Test interactions
- Test data flow
- More realistic scenarios

#### HTTP Testing
- Use HttpClientTestingModule
- Mock HTTP responses
- Test error cases
- Verify requests

### E2E Testing

#### Cypress (Recommended)
Modern E2E testing:
- Fast and reliable
- Time travel debugging
- Real browser testing
- Better DX than Protractor

##### Cypress Setup
- Install cypress
- Configure
- Write specs
- Run tests

##### Test Structure
- Visit pages
- Interact with elements
- Make assertions
- Test user flows

#### Testing Library
User-centric testing:
- Query by role, label, text
- Simulate user interactions
- Accessibility-focused
- Better tests

### Test Coverage

#### Coverage Reports
```bash
ng test --code-coverage
```
- Line coverage
- Branch coverage
- Function coverage
- Statement coverage

#### Coverage Goals
- Aim for 80%+ overall
- 100% for critical business logic
- 70%+ for components
- 90%+ for services

#### Coverage Tools
- Istanbul
- Code coverage reports
- HTML reports
- CI/CD integration

### Mocking Strategies

#### Mock Services
- Create mock implementations
- Use spy objects
- Return observables
- Simulate async operations

#### Mock HTTP
- HttpClientTestingModule
- Mock responses
- Test error handling
- Verify request details

#### Mock Router
- RouterTestingModule
- Mock navigation
- Test route guards
- Test navigation events

### Testing Best Practices

#### AAA Pattern
- **Arrange**: Set up test data
- **Act**: Execute the code
- **Assert**: Verify results

#### Test Naming
- Descriptive names
- Should/When/Then pattern
- Clear intent
- Easy to understand failures

#### Test Organization
- Group related tests (describe blocks)
- Setup in beforeEach
- Cleanup in afterEach
- Shared test utilities

#### Test Isolation
- No shared state
- Independent tests
- Can run in any order
- No test interdependencies

### Continuous Integration

#### CI Pipeline
- Run tests on every commit
- Run tests on PRs
- Block merge if tests fail
- Coverage reports

#### Test Automation
- Pre-commit hooks (Husky)
- Automated test runs
- Parallel execution
- Fast feedback

### Debugging Tests

#### Debug in Browser
- Chrome DevTools
- Breakpoints
- Step through code
- Inspect variables

#### Debug in IDE
- VS Code debugger
- Set breakpoints
- Watch variables
- Call stack

---

## 15. Build & Deployment

### Build Process

#### Development Build
```bash
ng build
```
- Not optimized
- Includes source maps
- Larger bundle size
- Faster build time

#### Production Build
```bash
ng build --configuration production
```
**Optimizations**:
- AOT compilation
- Tree shaking
- Minification
- Uglification
- Removal of dead code
- Optimization of assets
- Service worker generation (if PWA)

#### Build Configurations

##### Multiple Environments
- Development
- Staging
- Production
- Custom environments

##### Configuration Options
- File replacements
- Output path
- Base href
- Build optimizer
- Source maps
- Budget limits

### Build Output

#### Dist Folder Structure
```
dist/
└── project-name/
    ├── browser/
    │   ├── index.html
    │   ├── main.[hash].js
    │   ├── polyfills.[hash].js
    │   ├── runtime.[hash].js
    │   ├── styles.[hash].css
    │   └── assets/
    └── [other files]
```

#### Hashed Filenames
- Cache busting
- Unique names per build
- Browser caching strategy

### Deployment Strategies

#### Static Hosting

##### Firebase Hosting
```bash
npm install -g firebase-tools
firebase init
firebase deploy
```
**Features**:
- Fast CDN
- SSL certificate
- Custom domain
- Rewrites for SPA

##### Netlify
- Drag and drop deploy
- Git integration
- Automatic deploys
- Edge functions
- Form handling

##### Vercel
- Git integration
- Instant deploys
- Preview deployments
- Edge network
- Analytics

##### AWS S3 + CloudFront
- Static website hosting
- CDN distribution
- High scalability
- Custom configuration

##### GitHub Pages
- Free for public repos
- Custom domain support
- GitHub Actions integration

#### Server Deployment

##### Nginx
- Popular web server
- Serve static files
- Proxy to backend
- SSL termination
- Load balancing

Configuration needed:
- Root directory pointing to dist
- Try files for SPA routing
- Gzip compression
- Cache headers
- SSL certificates

##### Apache
- Alternative to Nginx
- .htaccess configuration
- Mod_rewrite for routing

##### Node.js Server
- Express server
- Serve Angular build
- API proxy
- SSR (Server-Side Rendering)

#### Container Deployment

##### Docker
Create Dockerfile:
- Multi-stage build
- Nginx base image
- Copy build files
- Configure nginx
- Expose port

##### Kubernetes
- Container orchestration
- High availability
- Auto-scaling
- Load balancing

### CI/CD Pipeline

#### GitHub Actions
Create workflow:
- Checkout code
- Install dependencies
- Run linter
- Run tests
- Build production
- Deploy to hosting

#### GitLab CI/CD
Similar pipeline:
- Build stage
- Test stage
- Deploy stage
- Environment variables

#### Jenkins
Traditional CI/CD:
- Build jobs
- Test jobs
- Deploy jobs
- Pipeline as code

#### Azure DevOps
Microsoft's platform:
- Build pipelines
- Release pipelines
- Artifact management

### Environment Variables in CI/CD

#### Secrets Management
- Store API keys securely
- Don't commit secrets
- Use CI/CD secret storage
- Environment-specific configs

#### Build-Time Variables
- API endpoints
- Feature flags
- Analytics IDs
- Third-party keys

### Pre-Deployment Checks

#### Automated Checks
- [ ] All tests passing
- [ ] Linting passing
- [ ] Build succeeds
- [ ] No console errors
- [ ] Bundle size within budget
- [ ] Security audit clean

#### Manual Checks
- [ ] Environment variables set
- [ ] API endpoints correct
- [ ] CORS configured
- [ ] Analytics working
- [ ] Error tracking configured
- [ ] Performance acceptable

### Deployment Process

#### Steps
1. Update version number
2. Create git tag
3. Run production build
4. Test build locally
5. Deploy to staging first
6. Test staging thoroughly
7. Deploy to production
8. Verify production
9. Monitor for errors

### Rollback Strategy

#### Quick Rollback
- Keep previous build
- Nginx/CDN configuration
- Quick switch if issues
- Database migrations consideration

### Post-Deployment

#### Monitoring
- Error rates
- Performance metrics
- User analytics
- API response times
- Resource usage

#### Health Checks
- Application loads
- API connectivity
- Critical features working
- No console errors

---

## 16. Production Checklist

### Performance Checklist

- [ ] Production build optimized
- [ ] Lazy loading implemented
- [ ] Bundle size analyzed and optimized
- [ ] Images optimized and lazy-loaded
- [ ] OnPush change detection used where possible
- [ ] Service worker configured (if PWA)
- [ ] Caching strategy implemented
- [ ] Compression enabled (gzip/brotli)
- [ ] CDN configured for static assets
- [ ] Performance budget defined and met
- [ ] Core Web Vitals passing
- [ ] No memory leaks (profiled)

### Security Checklist

- [ ] HTTPS enabled
- [ ] Content Security Policy configured
- [ ] XSS protection implemented
- [ ] CSRF protection in place
- [ ] Secrets stored securely (not in code)
- [ ] JWT tokens handled securely
- [ ] Input validation implemented
- [ ] Output encoding in place
- [ ] Dependency vulnerabilities checked (`npm audit`)
- [ ] Authentication implemented
- [ ] Authorization/permissions working
- [ ] Rate limiting on sensitive endpoints
- [ ] CORS configured correctly
- [ ] Security headers set (Helmet equivalent)

### Code Quality Checklist

- [ ] All tests passing
- [ ] Test coverage > 80%
- [ ] ESLint passing with no warnings
- [ ] Code reviewed
- [ ] No console.log statements
- [ ] No commented-out code
- [ ] Documentation updated
- [ ] Type safety enforced (strict mode)
- [ ] Error handling complete
- [ ] Loading states implemented
- [ ] Empty states designed

### SEO & Accessibility Checklist

- [ ] Meta tags configured
- [ ] Open Graph tags added
- [ ] Title service implemented
- [ ] Semantic HTML used
- [ ] Alt text on images
- [ ] ARIA labels where needed
- [ ] Keyboard navigation works
- [ ] Focus management implemented
- [ ] Color contrast sufficient
- [ ] Screen reader tested
- [ ] Sitemap generated
- [ ] robots.txt configured
- [ ] Structured data added (JSON-LD)

### User Experience Checklist

- [ ] Loading indicators on async operations
- [ ] Error messages user-friendly
- [ ] Success notifications implemented
- [ ] Form validation clear
- [ ] Responsive design works on all devices
- [ ] Touch targets appropriate size (mobile)
- [ ] Offline functionality (if PWA)
- [ ] Intuitive navigation
- [ ] Clear call-to-actions
- [ ] Consistent UI across app

### Monitoring & Analytics Checklist

- [ ] Error tracking configured (Sentry, etc.)
- [ ] Analytics implemented (GA4, etc.)
- [ ] Performance monitoring setup
- [ ] User behavior tracking
- [ ] Conversion tracking
- [ ] Custom events defined
- [ ] Error alerts configured
- [ ] Performance alerts set up
- [ ] Log aggregation in place
- [ ] Dashboard for monitoring

### Browser Support Checklist

- [ ] Tested on Chrome
- [ ] Tested on Firefox
- [ ] Tested on Safari
- [ ] Tested on Edge
- [ ] Tested on mobile browsers
- [ ] Polyfills for older browsers (if needed)
- [ ] Graceful degradation for unsupported features
- [ ] Browser compatibility documented

### Documentation Checklist

- [ ] README updated
- [ ] API documentation complete
- [ ] Setup instructions clear
- [ ] Environment variables documented
- [ ] Deployment guide written
- [ ] Architecture documented
- [ ] Code comments for complex logic
- [ ] Changelog maintained
- [ ] User guides (if needed)

### Infrastructure Checklist

- [ ] Domain configured
- [ ] SSL certificate installed
- [ ] DNS configured correctly
- [ ] CDN configured
- [ ] Backup strategy in place
- [ ] Disaster recovery plan
- [ ] Scaling strategy defined
- [ ] Database optimized (backend)
- [ ] CI/CD pipeline working
- [ ] Staging environment matches production

### Legal & Compliance Checklist

- [ ] Privacy policy linked
- [ ] Terms of service linked
- [ ] Cookie consent implemented (if EU users)
- [ ] GDPR compliance (if applicable)
- [ ] Data retention policy implemented
- [ ] License information correct
- [ ] Third-party licenses documented

---

## Additional Production Packages

### PWA Enhancement
```bash
ng add @angular/pwa
```
- Service worker
- App manifest
- Offline support
- Install prompt
- Push notifications

### SEO
```bash
npm install @angular/platform-server
```
- Server-Side Rendering (SSR)
- Better SEO
- Faster initial load
- Social media previews

### Analytics
```bash
npm install @angular/fire
```
For Firebase Analytics, or:
```bash
npm install ngx-google-analytics
```
For Google Analytics 4

---

## Quick Reference Commands

### Development
```bash
ng serve                        # Start dev server
ng serve --open                 # Open browser
ng serve --port 4300            # Custom port
ng serve --host 0.0.0.0         # Allow external access
```

### Building
```bash
ng build                        # Development build
ng build --configuration production  # Production build
ng build --watch                # Watch mode
```

### Testing
```bash
ng test                         # Run unit tests
ng test --code-coverage        # With coverage
ng e2e                         # Run E2E tests
```

### Code Generation
```bash
ng generate component name              # Component
ng generate service name                # Service
ng generate module name --routing       # Module with routing
ng generate guard name                  # Guard
ng generate interceptor name            # Interceptor
ng generate pipe name                   # Pipe
ng generate directive name              # Directive
```

### Linting
```bash
ng lint                         # Run ESLint
ng lint --fix                   # Auto-fix issues
```

### Updates
```bash
ng update                       # Check for updates
ng update @angular/cli @angular/core    # Update Angular
ng update --all                 # Update all packages
```

---

## Notes & Best Practices

1. **Always use lazy loading** - Load routes and modules on demand
2. **Implement OnPush** - For better performance in smart components
3. **Unsubscribe observables** - Prevent memory leaks
4. **Use async pipe** - Automatic subscription management
5. **Type everything** - Leverage TypeScript fully
6. **Test as you develop** - Don't save testing for last
7. **Keep components small** - Single Responsibility Principle
8. **Use smart/dumb pattern** - Container vs presentational components
9. **Avoid logic in templates** - Keep templates simple
10. **Regular dependency updates** - Security and features
11. **Monitor bundle size** - Keep it lean
12. **Accessibility first** - Design for everyone
13. **Mobile-first approach** - Start with mobile design
14. **Security** - Always validate and sanitize inputs
15. **Documentation** - Document complex logic and decisions

---

**Document Version**: 1.0  
**Last Updated**: October 2025  
**Framework**: Angular 17.x/18.x  
**Recommended UI**: Angular Material
