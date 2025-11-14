# agda-setup-action

GitHub composite action to install Agda from the official deployed binaries and optionally the Agda standard library.

## Features

- Installs Agda from official GitHub releases.
- Optional installation of the Agda standard library.
- Cross-platform support:
  - Ubuntu (latest)
  - Windows (latest)
  - macOS (latest, including macOS-15-intel)
- Outputs the path to the Agda executable and Agda application directory.

## Usage

### Basic Usage (Agda only)

```yaml
- name: Setup Agda
  uses: agda/agda-setup-action@v1
  with:
    agda-version: '2.8.0'
```

### With Standard Library

```yaml
- name: Setup Agda with stdlib
  uses: agda/agda-setup-action@v1
  with:
    agda-version: '2.8.0'
    agda-stdlib-version: '2.3'
```

### Using Outputs

```yaml
- name: Setup Agda
  id: setup-agda
  uses: agda/agda-setup-action@v1
  with:
    agda-version: '2.8.0'
    agda-stdlib-version: '2.3'

- name: Use Agda
  run: |
    echo "Agda path: ${{ steps.setup-agda.outputs.agda-path }}"
    echo "Agda dir: ${{ steps.setup-agda.outputs.agda-dir }}"
    agda --version
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `agda-version` | Version of Agda to install (e.g., `2.8.0`) | Yes | - |
| `agda-stdlib-version` | Version of Agda standard library to install (e.g., `2.3`). If not specified, stdlib will not be installed. | No | `''` |

## Outputs

| Name | Description |
|------|-------------|
| `agda-path` | Path to the Agda executable |
| `agda-dir` | Path to the Agda application directory |

## Example Workflow

```yaml
name: Build with Agda

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v5
      
      - name: Setup Agda
        uses: agda/agda-setup-action@v1
        with:
          agda-version: '2.8.0'
          agda-stdlib-version: '2.3'
      
      - name: Build Agda files
        run: agda Main.agda
```

## Platform Support

This action supports the following platforms:
- `ubuntu-latest`
- `windows-latest`
- `macos-latest`
- `macos-15-intel`

The action automatically detects the platform and downloads the appropriate binary for your runner.

## License

This project is licensed under the same terms as Agda itself.
