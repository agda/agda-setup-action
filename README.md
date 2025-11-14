# agda-setup-action

GitHub composite action to install Agda from the official deployed binaries and optionally the Agda standard library.

It supports Agda 2.8.0 (and up).
For older Agda versions, please use [`wenkokke/setup-agda`](https://github.com/wenkokke/setup-agda).

## Features

- Installs Agda from official GitHub releases.
- Optional installation of the Agda standard library.
- Cross-platform support:
  - Linux
  - Windows
  - macOS (ARM and x86)
- Outputs the path to the Agda executable and Agda application directory.

## Usage

### Basic Usage (default Agda version)

```yaml
- name: Setup Agda
  uses: agda/agda-setup-action@v1
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

| Name                  | Description                                                                                                     | Required | Default |
|-----------------------|-----------------------------------------------------------------------------------------------------------------|----------|---------|
| `agda-version`        | Version of Agda to install (e.g., `2.8.0`)                                                                      | Yes      | `2.8.0` |
| `agda-stdlib-version` | Version of Agda standard library to install (e.g., `2.3`). If not specified, the library will not be installed. | No       | `''`    |

If the standard library is installed, it is also added to the default libraries:

- The path to `standard-library.agda-lib` is added to the `libraries` file in the Agda directory (`${{ steps.setup-agda.outputs.agda-dir }}`).
- The string `standard-library-${{ agda-stdlib-version }}` is added to the `defaults` file there.

## Outputs

| Name        | Description                            |
|-------------|----------------------------------------|
| `agda-path` | Path to the Agda executable            |
| `agda-dir`  | Path to the Agda application directory |

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

- Linux, e.g. `ubuntu-latest`
- Windows, e.g. `windows-latest`
- macOS ARM, e.g. `macos-latest`
- macOS x86, e.g. `macos-15-intel`

The action automatically detects the platform and downloads the appropriate binary for your runner.

## License

This project is licensed under the BSD 3-clause license.
