# FileSweeper v1.0.1 - Desktop Build Guide

This guide explains how to build FileSweeper as a native Windows desktop application using Electron.

## Prerequisites

1. **Node.js** (v18 or higher) - https://nodejs.org
2. **Git** - https://git-scm.com
3. **Windows Build Tools** (for Windows):
   ```bash
   npm install --global windows-build-tools
   ```

## Setup Instructions

### 1. Clone or Download the Project

Download the project files to your local Windows PC.

### 2. Install Dependencies

Open a terminal in the project folder and run:

```bash
npm install
```

### 3. Add Electron Scripts to package.json

Add these scripts to your `package.json` file:

```json
{
  "main": "electron/main.js",
  "scripts": {
    "electron:dev": "concurrently \"cross-env NODE_ENV=development npm run dev\" \"wait-on http://localhost:5000 && cross-env NODE_ENV=development electron .\"",
    "electron:build": "npm run build && electron-builder --config electron-builder.json"
  }
}
```

### 4. Build the Web App

First, build the React frontend:

```bash
npm run build
```

### 5. Run in Development Mode

To test the Electron app during development:

```bash
npm run electron:dev
```

This will:
- Start the Vite dev server on port 5000
- Launch the Electron app pointing to the dev server
- Enable hot reload for faster development

### 6. Build for Production

To create the Windows installer:

```bash
npm run electron:build
```

This will create:
- `release/FileSweeper-1.0.1-x64.exe` - Windows installer
- `release/FileSweeper-1.0.1-x64-portable.exe` - Portable version (no install needed)

## Available Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start web development server |
| `npm run build` | Build web app for production |
| `npm run electron:dev` | Run Electron in development mode |
| `npm run electron:build` | Build Windows installer |

## Features in Desktop Version

The Electron version includes these exclusive features:

1. **Real File System Access** - Scan actual folders on your PC
2. **Native Folder Browser** - Use Windows folder picker dialog
3. **Drive Detection** - Automatically detect all drives (C:, D:, E:, etc.)
4. **Recycle Bin Integration** - Safely delete files to Recycle Bin
5. **Direct File Operations** - Backup, archive, and delete real files
6. **Frameless Window** - Custom Windows title bar with native controls

## Project Structure

```
filesweeper/
├── electron/
│   ├── main.js          # Electron main process
│   └── preload.js       # Secure bridge to renderer
├── client/src/
│   └── lib/electron.ts  # Frontend Electron API wrapper
├── electron-builder.json # Build configuration
└── ELECTRON_BUILD.md    # This file
```

## Troubleshooting

### "Cannot find module 'electron'"
Run `npm install` to ensure all dependencies are installed.

### Build fails on Windows
Make sure you have Windows Build Tools installed:
```bash
npm install --global windows-build-tools
```

### App won't start
Check that port 5000 is not in use by another application.

## Security Notes

- The app uses `contextIsolation: true` for security
- File system access is only exposed through the preload script
- No direct Node.js access from the renderer process

## Support

For issues or feature requests, please open an issue on the repository.
