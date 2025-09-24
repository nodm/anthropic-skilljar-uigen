# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` - Start development server with Turbopack
- `npm run build` - Build the Next.js application for production
- `npm run lint` - Run ESLint to check code quality
- `npm run test` - Run tests with Vitest
- `npm run setup` - Install dependencies, generate Prisma client, and run database migrations
- `npm run db:reset` - Reset the database (force migrate reset)

## Architecture Overview

UIGen is an AI-powered React component generator that uses a virtual file system to create and preview components without writing files to disk. The application has several key architectural components:

### Virtual File System

The core of the application is built around a `VirtualFileSystem` class (`src/lib/file-system.ts`) that manages files in memory. This system:

- Provides file operations (create, read, update, delete, rename) without touching the actual filesystem
- Serializes/deserializes file state for persistence in the database
- Integrates with AI tools through the `FileSystemContext` to handle code generation and modifications

### AI Integration & Tools

- Uses Anthropic Claude via Vercel AI SDK (`src/app/api/chat/route.ts`)
- Implements custom tools for file manipulation:
  - `str_replace_editor` tool for creating and editing files
  - `file_manager` tool for renaming and deleting files
- AI responses trigger file system updates through tool calls handled in `FileSystemContext`

### Code Transformation & Preview

The JSX transformer (`src/lib/transform/jsx-transformer.ts`) handles:

- Babel transformation of JSX/TSX code for browser execution
- Import resolution and creation of ES module import maps
- Missing import detection and placeholder module generation
- CSS processing and style injection
- Real-time preview generation with error boundaries

### Project Persistence

- Uses Prisma with SQLite for data persistence (`prisma/schema.prisma`)
- Projects store serialized messages and file system state as JSON
- Supports both authenticated users and anonymous sessions
- Anonymous work is tracked but not persisted across sessions

### Component Structure

- `src/components/editor/` - File tree and code editor components
- `src/components/chat/` - Chat interface for AI interaction
- `src/components/preview/` - Live preview rendering with iframe sandboxing
- UI components built with Radix UI and Tailwind CSS v4

### Context Providers

Two main contexts manage application state:

- `FileSystemContext` - Manages virtual file system operations and AI tool integration
- `ChatContext` - Handles chat messages and AI communication

The application follows a client-side architecture where all file operations happen in-memory, making it possible to generate, edit, and preview React components without any server-side file system access.

### Development Best Practices

- Follow the [Conventional Commits specification](https://www.conventionalcommits.org/en/v1.0.0/#specification) for the commit messages.
- Use comments sparingly. Only comment complex code.

### Database

- The database schema is defined in the @prisma/schema.prisma file. Reference it anytime you need to understand the structure of data stored in the database.
