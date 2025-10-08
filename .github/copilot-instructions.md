# Copilot Instructions for Bind Mount Applications

## Project Overview
This is a Docker learning project featuring a simple Todo app with Node.js/Express frontend and MongoDB backend. The codebase demonstrates containerized application development with Docker Compose.

## Architecture Patterns

### Service Architecture
- **App Service**: Node.js Express server (`app/server.js`) on port 3001
- **Database Service**: MongoDB (`todo-database`) on custom port 27018
- **Connection**: App connects to MongoDB via service name `todo-database:27018`

### Key Files & Structure
```
app/
├── server.js              # Main Express server, sets up EJS and routes  
├── config/keys.js         # Database connection config (uses service name)
├── models/Todo.js         # Mongoose schema for todos
├── routes/front.js        # All HTTP routes (GET /, POST /, POST /todo/destroy)
└── views/todos.ejs        # Single EJS template with Bootstrap
```

## Development Workflows

### Docker Development
- **Start**: `docker compose up -d` (app runs on http://localhost:3001)  
- **Development Mode**: Container uses `npm run dev` with nodemon for live reload
- **Bind Mounts**: Commented out in `compose.yaml` - uncomment volumes for live code changes

### Key Conventions
- **Database Config**: Always use Docker service names (`todo-database:27018`), not localhost
- **Port Mapping**: App runs on 3001 internally and externally  
- **MongoDB Port**: Uses non-standard 27018 to avoid conflicts
- **EJS Templates**: Moment.js available globally via `app.locals.moment`

### Model Patterns
- Single Mongoose model exports as `module.exports = Todo = mongoose.model('todos', TodoSchema)`
- Schema uses `created_at` with `Date.now()` default
- Routes use async/await for database operations

### Route Structure
- All routes in single file (`routes/front.js`)
- GET `/` - fetches todos and renders EJS template
- POST `/` - creates new todo and redirects
- POST `/todo/destroy` - deletes todo by `_key` field

## Docker Specifics
- **Dockerfile**: Uses Node 19.5.0 Alpine with multi-stage build optimizations
- **Dependencies**: Installed via bind mounts for caching efficiency
- **Runtime User**: Runs as non-root `node` user
- **Links**: Uses legacy Docker `links` for service discovery

## Quick Start for AI Agents
1. Database operations always go through Mongoose models
2. All HTML rendering uses EJS with Bootstrap 4.3.1 and Font Awesome
3. Form submissions are POST requests with `body-parser` URL-encoded parsing
4. Error handling is minimal - check `server.js` and route files for patterns
5. No authentication, testing, or API endpoints - pure server-rendered CRUD app