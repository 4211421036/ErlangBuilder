# Erlang Build and Run Action

GitHub Action to compile Erlang files to executable (.exe) or run them in debug mode. This action will automatically detect and download the dependencies needed by your Erlang code.

## Features

- ✅Automatically detects dependencies** from your Erlang files
- ✅Download dependencies** via Hex package manager
- ✅Handles various Erlang file types** dynamically
- ✅Compile to executable** with all dependencies
- ✅Debugging mode for development
- ✅Supports**multiple files** in one project
- ✅ Automatically create build directories


## Usage

Create a workflow file `.github/workflows/erlang-build.yml` in your repository with the following content:

```yaml
name: Build Erlang Project

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Erlang Project
        uses: 4211421036/erlang-build-action@v1.0.0
        with:
          erlang-source: './src'  # Path ke file atau direktori Erlang Anda
          build-mode: 'build'     # 'build' untuk executable, 'debug' untuk debugging
          output-dir: './build'   # Direktori output (akan dibuat otomatis)
          erts-version: '25.0'    # Versi Erlang/OTP (opsional)
          hex-packages: 'cowboy,jsx,lager'  # Tambahan Hex packages (opsional)
```

## Inputs

| Input         | Description | Required | Default |
|---------------|--------------------------------------------------|-------|------------|
| erlang-source | Path to Erlang file or directory | Yes | - |
| build-mode | Mode ('build' or 'debug') | Yes | 'build' |
| output-dir | Directory for build output | No | './build' |
| erts-version | Erlang/OTP version | No | '25.0' |
| hex-packages | Additional hex packages (comma separated) | No | '' |

## How it works


1.**Dynamic Dependency Analysis**:
- Action will automatically analyze Erlang files to find modules that are used
- Detects include files and module calls


2.**Dependency Management**:
- Uses rebar3 to download required dependencies.
- Supports adding additional hex packages via parameters


3.**Build Process**:
- Compiles all found Erlang files
- Creates a complete Erlang application with dependencies
- Generates an executable that can be run independently


4.**Debug Mode**:
- Runs Erlang files in debug mode
- Automatically detects start/0 functions if present

## Example

### Compile One File with Dependencies

```yaml
- name: Build Single Erlang File
  uses: 4211421036/erlang-build-action@v1.0.0
  with:
    erlang-source: './src/my_web_app.erl'
    build-mode: 'build'
    hex-packages: 'cowboy,jsx'
```

### Compile All Files in a Directory

```yaml
- name: Build All Erlang Files
  uses: 4211421036/erlang-build-action@v1.0.0
  with:
    erlang-source: './src'
    build-mode: 'build'
```

### Debug Mode with Additional Dependencies

```yaml
- name: Debug Erlang Application
  uses: 4211421036/erlang-build-action@v1.0.0
  with:
    erlang-source: './src'
    build-mode: 'debug'
    hex-packages: 'lager,jsx,cowlib'
```

## Noted

- This action uses Erlang/OTP and rebar3 for dependency management.
- Build mode will generate an executable package containing the Erlang runtime
- Automatic dependency detection works best for standard modules and popular hex packages
- For custom dependencies, use the `hex-packages` parameter
