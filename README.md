# Useful scripts

This project provides a set of utility that were once useful to me and, hopefully, can be useful to you too. All scripts are designed to be portable and work in standard Unix-like environments.

## Table of contents

1. [Getting Started](#getting-started)
2. [Scripts](#scripts)
    1. [`bump-version`](#bump-version)
    2. [`colors`](#colors)
    3. [`show-files`](#show-files)
    4. [`spaces2underscores`](#spaces2underscores)
    5. [`underscores2spaces`](#underscores2spaces)
    6. [`waitfor`](#waitfor)
3. [Contributing](#contributing)
4. [Code of Conduct](#code-of-conduct)
5. [License](#license)

---

## Getting Started <a name="getting-started"></a>

1. Clone the repository:

```bash
git clone git@github.com:maxbarsukov/useful-scripts.git
cd useful-scripts
```

2. Add the `bin` directory to your PATH:

```bash
# Make scripts executable
chmod +x bin/*

# For temporary access:
export PATH="$PATH:$(pwd)/bin"

# For permanent access (add to ~/.bashrc or ~/.zshrc):
echo 'export PATH="$PATH:'$(pwd)'/bin"' >> ~/.bashrc
source ~/.bashrc
```

3. Run any script directly from your terminal.

## Scripts <a name="scripts"></a>

### `bump-version` <a name="bump-version"></a>

**Purpose**: Updates project version with semantic versioning and creates Git commit/tag.
**Use Case**: Automate version management during release workflows.

```bash
bump-version <semantic-version>
```

- Updates version file.
- Creates commit with standardized message.
- Generates annotated Git tag.

#### Example

```bash
bump-version 1.2.0
```

### `colors` <a name="colors"></a>

**Purpose**: Displays a 256-color palette in terminal.
**Use Case**: Test terminal color support and choose color codes for scripts.

```bash
colors
```

### `show-files` <a name="show-files"></a>

#### Purpose

`show-files` is a file inspection tool that recursively displays file contents while respecting ignore rules (`.gitignore`) and handling various file types. It's designed for:

- Quickly auditing project files
- Inspecting configuration files in directories
- Reviewing codebases with proper syntax highlighting
- Troubleshooting file content issues in complex directories

**Use Case**: Inspect project files while respecting ignore rules and handling large directories.

```bash
show-files [OPTIONS] [FILE_OR_DIRECTORY]
```

#### Dependencies

| Dependency      | Installation                     | Required For                          |
|-----------------|----------------------------------|---------------------------------------|
| `tree`          | `sudo apt install tree`          | Directory tree visualization          |
| `bat`           | `sudo apt install bat`           | Syntax highlighting                   |
| `grep`          | Preinstalled on most systems     | Pattern matching                      |
| `find`          | Preinstalled on most systems     | File discovery                        |
| `numfmt`        | Part of GNU coreutils            | Human-readable size formatting        |
| `stat`          | Part of GNU coreutils            | File metadata                         |
| `sha256sum`     | Part of GNU coreutils            | File checksums                        |
| `less`          | Preinstalled on most systems     | Paged output (`-p` option)            |
| `xclip`         | `sudo apt install xclip`         | Clipboard access on Linux (-C option) |
| `pbcopy`        | Preinstalled on macOS            | Clipboard access on macOS (-C option) |

#### Installation Guide

1. Clone the repository as shown in the Installation section above
2. Ensure dependencies are installed:
```bash
sudo apt update
sudo apt install tree bat
```
3. Make the script executable:
```bash
chmod +x bin/show-files
```

#### Usage Examples

**Basic Usage**

```bash
# Show all files in current directory (respects .gitignore)
show-files

# Inspect specific directory
show-files /path/to/project

# Inspect single file
show-files config/settings.conf
```

**Recommended Settings**

```bash
# Enable color, metadata, and tree view (5KB file size limit)
show-files -R
```

**Advanced Filtering**

```bash
# Only show Python files with metadata
show-files -i '*.py' --meta

# Exclude log files and files larger than 1MB
show-files -e '*.log' -s 1M

# Show files modified in the last week
show-files --newer-than lastweek
```

**Interactive Mode**

```bash
# Prompt before showing each file
show-files --interactive
```

**Output Control**

```bash
# Save output to file
show-files -o project_snapshot.txt

# View with pager (less)
show-files -p

# Copy output to clipboard (requires xclip/pbcopy)
show-files -C
```

#### Full Options Reference

```bash
show-files [OPTIONS] [FILE_OR_DIRECTORY]
```

| Option | Description | Example |
|--------|-------------|---------|
| `-R`, `--recommended` | Recommended settings (color, tree, metadata) | `show-files -R` |
| `-l`, `--long` | Show full content of large files | `show-files -l` |
| `-p`, `--less` | Use pager for output | `show-files -p` |
| `-i`, `--include` | Process only matching files | `show-files -i '*.sh'` |
| `-e`, `--exclude` | Exclude matching files | `show-files -e '*.log'` |
| `-s`, `--max-size` | Skip larger files | `show-files -s 10M` |
| `-o`, `--output` | Write to file | `show-files -o output.txt` |
| `-c`, `--color` | Enable color output | `show-files -c` |
| `-t`, `--tree` | Show directory tree | `show-files -t` |
| `--meta` | Show file metadata | `show-files --meta` |
| `--checksum` | Include file checksum | `show-files --checksum` |
| `--newer-than` | Show files modified after date | `show-files --newer-than 2023-01-01` |
| `--interactive` | Prompt before each file | `show-files --interactive` |
| `-h`, `--help` | Show help message | `show-files -h` |

### `spaces2underscores` <a name="spaces2underscores"></a>

**Purpose**: Bulk rename files by replacing spaces with underscores.

#### Example

```bash
spaces2underscores ~/docs
```

### `underscores2spaces` <a name="underscores2spaces"></a>

**Purpose**: Bulk rename files by replacing underscores with spaces.

#### Example

```bash
underscores2spaces ~/data
```

### `waitfor` <a name="waitfor"></a>

**Purpose**: Monitors a process and executes a predefined action when the process terminates.
**Use Case**: Automate cleanup tasks or shutdown sequences after long-running processes complete.

```bash
waitfor [pid]
```

**Configuration**: Edit these variables in the script:

```bash
interval=300           # Check every 300 seconds (5 minutes)
action="shutdown now"  # Action to perform when process ends
```

#### Example

```bash
# Edit script
action='curl -s "https://api.telegram.org/botTOKEN/sendMessage?chat_id=ID&text=Backup+is+done"'
# ...

# Start a long-running process in background
rsync -avz /data/ user@backup-server:/backups/ &
# Monitor its PID
waitfor $!
```

---

## Contributing <a name="contributing"></a>

Before creating your PR, we strongly encourage you to read the repository's corresponding [`CONTRIBUTING.md`](https://github.com/maxbarsukov/useful-scripts/blob/master/CONTRIBUTING.md).

## Code of Conduct <a name="code-of-conduct"></a>

This project is intended to be a safe, welcoming space for collaboration, and everyone interacting in the project's codebases, issue trackers, chat rooms and mailing lists is expected to adhere to the [code of conduct](https://github.com/maxbarsukov/useful-scripts/blob/master/CODE_OF_CONDUCT.md).

## License <a name="license"></a>

The project is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

**Leave a star :star: if you find this project useful.**
