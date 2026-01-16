# FileSweeper v1.0.1

## Overview

FileSweeper v1.0.1 is a file storage management application that helps users find old, large, or unused files and take action on them (backup, archive, or delete) to reclaim disk space. The app provides a dashboard interface for scanning directories, filtering results by file age/size/type, and batch-processing selected files.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight router)
- **State Management**: TanStack React Query for server state
- **UI Components**: shadcn/ui component library built on Radix UI primitives
- **Styling**: Tailwind CSS v4 with CSS variables for theming
- **Animations**: Framer Motion for transitions
- **Build Tool**: Vite with custom plugins for Replit integration

The frontend follows a component-based architecture with:
- Pages in `client/src/pages/`
- Reusable UI components in `client/src/components/ui/`
- Feature-specific components in `client/src/components/file-manager/`
- Custom hooks in `client/src/hooks/`
- API and utility functions in `client/src/lib/`

### Backend Architecture
- **Runtime**: Node.js with Express
- **Language**: TypeScript with ES modules
- **API Design**: RESTful endpoints under `/api` prefix
- **Database ORM**: Drizzle ORM with PostgreSQL dialect
- **Schema Validation**: Zod with drizzle-zod integration

The backend serves both the API and static files in production. In development, Vite's dev server handles the frontend with HMR support.

### Data Storage
- **Database**: PostgreSQL
- **Schema Location**: `shared/schema.ts` (shared between frontend and backend)
- **Migrations**: Generated via `drizzle-kit push`

Key database tables:
- `scan_sessions`: Tracks scan operations with filters and results
- `scanned_files`: Individual files discovered during scans
- `file_actions`: History of actions taken on files
- `users`: User accounts (basic structure present)

### Build System
- **Frontend**: Vite builds to `dist/public`
- **Backend**: esbuild bundles server code to `dist/index.cjs`
- **Development**: `tsx` for TypeScript execution without compilation
- **Production**: Single Node.js process serves both API and static assets

## External Dependencies

### Database
- **PostgreSQL**: Required via `DATABASE_URL` environment variable
- **Connection**: Uses `pg` driver with connection pooling
- **Sessions**: `connect-pg-simple` for session storage

### UI Framework
- **Radix UI**: Complete set of accessible primitives (dialog, dropdown, tabs, etc.)
- **shadcn/ui**: Pre-styled component library using Radix + Tailwind

### Key Libraries
- **TanStack Query**: Async state management and caching
- **date-fns**: Date manipulation and formatting
- **Framer Motion**: Animation library
- **Zod**: Runtime schema validation
- **drizzle-orm**: Type-safe database queries

### Replit-Specific
- `@replit/vite-plugin-runtime-error-modal`: Error overlay in development
- `@replit/vite-plugin-cartographer`: Development tooling
- `@replit/vite-plugin-dev-banner`: Development environment indicator

## Electron Desktop App

FileSweeper includes foundation for building as a native Windows desktop application using Electron.

### Electron Structure
- `electron/main.js`: Main Electron process with file system access
- `electron/preload.js`: Secure bridge between main and renderer processes
- `client/src/lib/electron.ts`: TypeScript wrapper for Electron APIs
- `electron-builder.json`: Build configuration for Windows/Mac/Linux installers

### Desktop Features
When running as an Electron app:
- Real file system access (scan actual Windows folders)
- Native folder picker dialog
- Drive detection (C:, D:, E:, etc.)
- Recycle Bin integration
- Direct file backup/archive/delete operations
- Frameless window with custom title bar

### Build Instructions
See `ELECTRON_BUILD.md` for detailed instructions on building the Windows installer.