# ⚡ Lightning CLI (ln-cli)

A powerful command-line tool for building Lightning firmware applications with remote build capabilities, incremental builds, and real-time streaming output.

`ln-cli` running a cloud firmware build:

![](https://github.com/user-attachments/assets/68b39d53-881f-439d-b9b6-45c8368e5af5)

Website coming soon.

![](https://github.com/user-attachments/assets/e157bbb0-dcb9-41a5-a12d-719810aa1d87)


## MCU Z1 Status
*November 28th, 2026*

| Feature | HW Design | Firmware Support | Code Example | Integration Test |
|---------|-----------|------------------|--------------|------------------|
| UART | ✅ Complete | ✅ Complete | ✅ Complete | 80% |
| GPIO | ❌ Not Started | ✅ Complete | ❌ Not Started | ❌ Not Started |
| SPI | ❌ Not Started | ✅ Complete | ❌ Not Started | ❌ Not Started |
| I2C | ❌ Not Started | ✅ Complete | ❌ Not Started | ❌ Not Started |
| ADC | ❌ Not Started | ✅ Complete | ❌ Not Started | ❌ Not Started |
| PWM | ✅ Complete | ✅ Complete | ⏳ Planned | ⏳ Planned |
| Timer | ✅ Complete | ✅ Complete | ⏳ Planned | ⏳ Planned |
| RTC | ✅ Complete | ✅ Complete | ⏳ Planned | ⏳ Planned |
| WDT | ✅ Complete | ✅ Complete | ⏳ Planned | ⏳ Planned |
| USB | ❌ Not Started | ❌ Not Started | ❌ Not Started | ❌ Not Started |
| CAN | ⏳ Planned | ❌ Not Started | ❌ Not Started | ❌ Not Started |
| CAN-FD | ⏳ Planned | ❌ Not Started | ❌ Not Started | ❌ Not Started |
| Flash | ❌ Not Started | ❌ Not Started | ❌ Not Started | ❌ Not Started |

**Legend:**
- ✅ Complete
- 🚧 In Progress
- ⏳ Planned
- ❌ Not Started


## Features

✨ **Remote Building** - Submit builds to a remote server and stream output in real-time  
🔄 **Incremental Builds** - Reuse build directories for faster compilation  
🎨 **Colored Output** - Beautiful ANSI-colored build logs  
🔐 **Authentication** - Secure API access with token-based auth  
🧹 **Smart Cleaning** - Clean build artifacts locally and on server  
📦 **Single Binary** - No dependencies, just download and run  

## Quick Start

### 1. Download

Download the latest release for your platform:

- **Linux (amd64)**: `ln-cli-linux-amd64-vX.X.X.tar.gz`
- **Linux (arm64)**: `ln-cli-linux-arm64-vX.X.X.tar.gz`
- **macOS (Intel)**: `ln-cli-darwin-amd64-vX.X.X.tar.gz`
- **macOS (Apple Silicon)**: `ln-cli-darwin-arm64-vX.X.X.tar.gz`
- **Windows (amd64)**: `ln-cli-windows-amd64-vX.X.X.zip`
- **Windows (arm64)**: `ln-cli-windows-arm64-vX.X.X.zip`

### 2. Extract and Run

**Linux/macOS:**
```bash
tar -xzf ln-cli-*.tar.gz
chmod +x ln-cli-*
./ln-cli-linux-amd64 version
```

**Windows:**
```powershell
# Extract the zip file
# Run in PowerShell or CMD
.\ln-cli-windows-amd64.exe version
```

### 3. Authenticate

Get a token from your build server administrator and save it:

```bash
ln-cli auth <your-token>
```

Or enter it interactively:
```bash
ln-cli auth
# Enter token: <paste-token-here>
```

### 4. Create a Project

```bash
ln-cli create hello_world
cd hello_world
```

This creates:
- `hello_world.yaml` - Configuration file
- `hello_world.cpp` - C++ source template

### 5. Build

```bash
ln-cli build
```

The CLI will:
- Find your YAML config and C/C++ sources
- Upload them to the remote build server
- Stream colored build output in real-time
- Cache for incremental rebuilds

## Commands

### `ln-cli auth`

Authenticate with the build server and save token locally.

```bash
# With token argument
ln-cli auth <token>

# With flag
ln-cli auth --token <token>

# Interactive prompt
ln-cli auth
```

### `ln-cli create`

Create a new Lightning application project.

```bash
ln-cli create <project-name>
```

Creates a directory with YAML config and C++ template.

### `ln-cli build`

Build your Lightning application remotely.

```bash
# Build current directory
ln-cli build

# Build specific project
ln-cli build ./my-project

# Custom timeout (default: 300s)
ln-cli build --timeout 600
```

**First build**: Creates a new build directory on the server  
**Subsequent builds**: Reuses the same build directory for faster incremental builds

### `ln-cli clean`

Clean build artifacts and cache.

```bash
# Clean current project
ln-cli clean

# Clean specific project
ln-cli clean ./my-project
```

Removes:
- Local build cache
- Remote build directory on the server

### `ln-cli version`

Display CLI version.

```bash
ln-cli version
```

## Project Structure

## Project Structure

A typical Lightning project:

```
my-project/
├── my-project.yaml          # Configuration
├── my-project.cpp           # Main source
├── additional.cpp           # Additional sources
└── header.h                 # Header files
```

The CLI automatically finds:
- One YAML/YML configuration file
- All `.c`, `.cpp`, `.cc`, `.cxx` source files
- All `.h`, `.hpp`, `.hh`, `.hxx` header files

## Examples

### Basic Workflow

```bash
# 1. Authenticate once
ln-cli auth eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# 2. Create project
ln-cli create led_blink
cd led_blink

# 3. Edit your code
vim led_blink.cpp

# 4. Build (first time - creates new build)
ln-cli build

# 5. Make changes and rebuild (incremental)
vim led_blink.cpp
ln-cli build

# 6. Clean when done
ln-cli clean
```

### Multiple Projects

Each project maintains its own build cache:

```bash
# Project A
cd project-a
ln-cli build

# Project B
cd ../project-b
ln-cli build
```

## Troubleshooting

### "Missing Authorization header" Error

Your token is missing or expired. Re-authenticate:

```bash
ln-cli auth <new-token>
```

### "No YAML configuration file found"

Make sure your project directory contains a `.yaml` or `.yml` file:

```bash
ls *.yaml *.yml
```

### Build Timeout

Increase the timeout for large projects:

```bash
ln-cli build --timeout 900  # 15 minutes
```

### Connection Issues

Check server endpoint:

```bash
# Test server health
curl http://localhost:8080/health
```

## Support

- **Issues**: [GitHub Issues](https://github.com/6COMG/flight/issues)
- **Documentation**: [Wiki](https://github.com/6COMG/flight/wiki)
- **Discussion**: [GitHub Discussions](https://github.com/6COMG/flight/discussions)

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and release notes.

---

Built with ⚡ by the Lightning [team](mailto:lab@6com.group)
