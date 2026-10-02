# Installation

## Requirements

- `curl`
- `make` (Linux/Mac): the most convenient way to install and check the repository is by using the various commands in `Makefile`. If it is not possible to use `make`, the relevant commands from the `Makefile` can be copied and run manually.
  : In particular, each of the basic commands installs the `uv` package manager automatically. Please see the instructions below for manual `uv` installation.

## Installing uv

The [uv](https://github.com/astral-sh/uv) package manager, which can manage python packages, python itself and virtual environments, can be installed manually by running:

```console
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## Installation

### Download the correct wheel

1. Open the **Releases** section of this repository and select the version of sofia that you need (typically the latest release).
2. Download the wheel that matches your environment:
   - **Operating System**: Linux, macOS or Windows
   - **Architecture**: x86_64, amd64 or arm64
   - **Python Version**: cp313 (Python 3.13 only; uv will install it automatically)

#### NOTE
If no available wheel matches your system, contact us so that we can provide a compatible build.

### Add the wheel to the project

1. Clone the repository or alternatively download the source files from the release.
2. Install the dependencies and package manager:

```console
make install
```

```console
uv sync
```

1. Navigate to the project directory and run:

```console
uv add <path_to_your_downloaded_wheel>
```

This installs sofia and its dependencies. The package will appear under [tool.uv.sources] in your pyproject.toml. To switch versions, simply repeat the command with a different wheel.

To verify the installed version:

```console
uv tree | grep sofia
```

```console
uv tree
```

### Test

To confirm that sofia is installed correctly, run:

```console
make test
```

```console
uv run test.py
```

### Examples

This repository contains some examples to get started using sofia, which can be found in the examples folder.
The recommended format of the examples uses [marimo](https://marimo.io/)) in the examples/marimo folder.

### Marimo (recommended)

To open the marimo version of the examples, the Marimo server can be started by running

```console
make marimo
```

```console
uv run --with marimo,ruff,watchdog,ty marimo --yes edit . --headless --no-token --watch
```

This should start a server and tell you which address to go to in your browser.
In the Workspace the examples should be listed and can be opened by clicking on them.

### Jupyter

Each example is also available in jupyter notebook format, located in examples/jupyter.
The Jupyter server can be started by running

```console
make jupyter
```

```console
uv run --with jupyter,marimo jupyter lab
```

## Developing and Contributing

If you have improvements to the interface, such as utility functions that simplify common tasks, please submit a pull request to this repository.

**Important**: If you plan to add/modify code specific to your own use cases, create a personal fork. Direct commits to this repository are visible to all contributing organizations.
