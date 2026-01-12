# Changelog

All notable changes to this project will be documented in this file.

## [1.0.0] - 2025-01-13

### Added
- Initial bulletproof release
- File to WebP Base64 conversion with comprehensive error handling
- Base64 to Image display with safety limits
- Full TypeScript support with type definitions
- Input validation and sanitization
- Resource cleanup and memory management
- Timeout protection for image loading
- Detailed error messages for debugging
- Safety limits to prevent browser freezing
- Support for PNG, JPEG, WebP, and GIF input formats
- High-quality image resizing with configurable options
- Exact byte size calculation for base64 strings
```

**`.npmignore`** file:
```
# Source files
src/
*.ts
!*.d.ts

# Config files
tsconfig.json
.eslintrc.*
.prettierrc

# Test files
test/
examples/
*.tgz

# Development
node_modules/
.git/
.github/
.vscode/

# OS files
.DS_Store
Thumbs.db