# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

LibreChat is an all-in-one AI conversations platform that integrates multiple AI models (OpenAI, Anthropic, Google, etc.) with advanced features like agents, tools integration, web search, code artifacts, and multimodal interactions. It's built as a full-stack application with React frontend and Express.js backend.

## Development Commands

### Frontend Development
- `npm run frontend:dev` - Start development server for React frontend
- `npm run frontend` - Build production frontend with all dependencies
- `npm run frontend:ci` - Build for CI environment
- `cd client && npm run typecheck` - Run TypeScript type checking for frontend
- `cd client && npm run test:ci` - Run frontend tests
- `npm run b:client:dev` - Development server using Bun
- `npm run b:client` - Build frontend with Bun

### Backend Development
- `npm run backend:dev` - Start backend in development mode with nodemon
- `npm run backend` - Start backend in production mode
- `npm run backend:stop` - Stop running backend
- `cd api && npm run test:ci` - Run backend tests
- `npm run b:api:dev` - Backend development with Bun watch mode
- `npm run b:api` - Production backend with Bun

### Package Building (Required before frontend builds)
- `npm run build:data-provider` - Build data provider package
- `npm run build:api` - Build API package  
- `npm run build:data-schemas` - Build data schemas package
- `npm run build:client-package` - Build client package

### Testing & Quality
- `npm run lint` - Run ESLint on all TypeScript/JavaScript files
- `npm run lint:fix` - Fix ESLint issues automatically
- `npm run format` - Format code with Prettier
- `npm run test:client` - Run client tests
- `npm run test:api` - Run API tests

### End-to-End Testing
- `npm run e2e` - Run Playwright E2E tests locally
- `npm run e2e:headed` - Run E2E tests with browser UI
- `npm run e2e:debug` - Debug E2E tests
- `npm run e2e:codegen` - Generate E2E test code

### User & Data Management
- `npm run create-user` - Create new user account
- `npm run invite-user` - Send user invitation
- `npm run list-users` - List all users
- `npm run delete-user` - Delete user account
- `npm run ban-user` - Ban user account
- `npm run add-balance` - Add balance to user account
- `npm run list-balances` - List user balances

## Architecture Overview

### Monorepo Structure
LibreChat uses npm workspaces with the following main components:

**Core Applications:**
- `/api/` - Express.js backend server with MongoDB/Redis
- `/client/` - React frontend with Vite build system  

**Shared Packages:**
- `/packages/data-provider/` - Shared data access layer and React Query hooks
- `/packages/data-schemas/` - Zod schemas and TypeScript types
- `/packages/api/` - Core API utilities and MCP (Model Context Protocol) support
- `/packages/client/` - Shared client components and utilities

### Key Directories

**Backend (`/api/`):**
- `server/` - Express server setup, routes, controllers, middleware
- `models/` - MongoDB Mongoose models
- `strategies/` - Authentication strategies (OAuth, LDAP, etc.)
- `utils/` - Backend utilities and helpers
- `cache/` - Caching layer implementation
- `db/` - Database configuration and connection

**Frontend (`/client/src/`):**
- `components/` - React UI components
- `hooks/` - Custom React hooks
- `store/` - State management (Recoil/Jotai)
- `routes/` - React Router configuration
- `utils/` - Frontend utilities
- `locales/` - Internationalization files
- `Providers/` - React context providers

### Build Dependencies
The packages have specific build order requirements:
1. `data-schemas` (TypeScript types/Zod schemas)
2. `data-provider` (uses data-schemas)
3. `api` (MCP and backend utilities)
4. `client-package` (shared client code)
5. Frontend application (uses all above)

## Configuration

### Environment Setup
- Copy `.env.example` to `.env` and configure required variables
- Primary config file: `librechat.yaml` (see `librechat.example.yaml`)
- MongoDB required (local or cloud)
- Redis recommended for production sessions/caching

### Key Environment Variables
- `MONGO_URI` - MongoDB connection string
- `HOST/PORT` - Server host and port (default: localhost:3080)
- `DOMAIN_CLIENT/DOMAIN_SERVER` - Domain configuration
- Various AI provider API keys (OpenAI, Anthropic, Google, etc.)

## Testing Strategy

- **Frontend**: Jest + React Testing Library
- **Backend**: Jest + Supertest
- **E2E**: Playwright with custom configuration
- **Type Safety**: TypeScript with strict mode across all packages

## Development Workflow

1. **Package Dependencies**: Always build packages before frontend: `npm run build:data-provider && npm run build:api && npm run build:data-schemas`
2. **Development**: Use `npm run frontend:dev` and `npm run backend:dev` concurrently
3. **Code Quality**: Run `npm run lint:fix` and `npm run format` before commits
4. **Testing**: Run relevant test suites for modified areas

## Special Features Integration

### Agents & Tools
- MCP (Model Context Protocol) server integration in `packages/api`
- Agent marketplace and sharing functionality
- Custom tool creation and management

### Multimodal & File Handling
- File upload/processing with multiple AI providers
- Image generation/editing capabilities
- Speech-to-text and text-to-speech integration

### Authentication
- Multiple OAuth providers (Google, GitHub, Discord, etc.)
- LDAP integration
- JWT-based session management

## Deployment Options

- Docker Compose (see `docker-compose.yml`)
- Cloud platforms (Railway, Zeabur, Sealos)
- Kubernetes (Helm charts in `/helm/`)
- Manual deployment with PM2 or similar

## Important Notes

- This is a security-conscious application - never commit API keys or sensitive data
- The application supports extensive customization through `librechat.yaml`
- User permissions and moderation features are built-in
- Internationalization is fully supported with 20+ languages