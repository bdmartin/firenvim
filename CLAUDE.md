# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Firenvim is a browser extension that transforms web textareas into Neovim instances. It consists of a webextension frontend communicating with a native Neovim backend via websockets.

## Build Commands

- `npm run build` - Full build for both Firefox and Chrome
- `npm run webpack` - Run webpack only
- `npm run clean` - Remove target directory
- `npm run pack` - Create Firefox extension package
- `npm run install_manifests` - Install native messaging manifests

### Browser-Specific Builds
- `"$(npm bin)/webpack --env=firefox"` - Firefox build only (faster iteration)
- `"$(npm bin)/webpack --env=chrome"` - Chrome build only (faster iteration)

## Testing Commands

- `npm run tests` - Run all tests (both Firefox and Chrome)
- `npm run test-firefox` - Firefox tests only
- `npm run test-chrome` - Chrome tests only
- `npm run jest` - Run Jest tests
- `npm run nyc` - Coverage analysis
- `npm run lint` - Lint the generated extension

## Architecture

Firenvim uses a multi-process webextension architecture:

### Core Processes
- **background.ts** - Background process that manages Neovim instances, handles browser shortcuts, and coordinates between other processes
- **content.ts** - Content script injected into web pages to detect editable elements and create Firenvim frames
- **frame.ts** - Neovim frame process that renders the editor and handles keyboard input
- **browserAction.ts** - Browser action popup for configuration and status

### Key Components
- **FirenvimElement.ts** - Main class managing the relationship between DOM elements and Neovim frames
- **KeyHandler.ts** - Handles keyboard input and forwards to Neovim
- **Neovim.ts** - WebSocket communication with Neovim backend
- **renderer.ts** - Renders Neovim's grid-based UI to HTML canvas
- **page.ts** - Communication proxy between frame and content scripts

### Vim/Lua Integration
- **autoload/firenvim.vim** - Main Vim plugin functions
- **lua/firenvim.lua** - Lua configuration and utilities
- **plugin/firenvim.vim** - Plugin initialization

## Development Notes

### Fast Iteration
Use browser-specific webpack commands for faster builds during development. Remember to reload the extension in your browser after each build.

### Native Messaging
The extension communicates with Neovim through native messaging. Run `npm run install_manifests` after building to set up the native messaging host.

### Testing Requirements
Tests require either Geckodriver (Firefox) or Chromedriver (Chrome) to be installed.

### Multi-Browser Support
The codebase supports both Firefox and Chrome through webpack configurations that handle browser-specific manifest and polyfill differences.

### Manifest V3 Migration
The extension has been migrated to Manifest V3 for Chrome compatibility:
- Background script runs as a service worker
- `browser_action` replaced with `action` API
- `web_accessible_resources` uses object format with matches
- Content Security Policy uses object format for `extension_pages`

## File Structure

- `src/` - TypeScript source code
- `autoload/` - Vim autoload functions
- `lua/` - Lua configuration modules
- `plugin/` - Vim plugin files
- `tests/` - Test files and test pages
- `target/` - Build output directory
- `static/` - Static assets (icons, etc.)