# Run command in another location

`shadow` is a tool that allows you to run shell commands in a different location (directory).

It allows a set command to run in multiple directories and cascade depending on command failure or success.

## Features

- Run a given command in multiple directories
- Run a command if the previous fails
- Run a command if the previous success

## Installation

```bash
git clone https://codeberg.org/aocoronel/shadow
cd shadow
mv src/shadow $HOME/.local/bin
chmod +x "$HOME/.local/bin/shadow"
```

## Usage

Base usage shadow:

```bash
shadow destination_dir/ -exec mkdir subdirectory
```

Run command in multiple directories:

```bash
shadow destination_dir1/ destination_dir2/ -exec mkdir subdirectory
```

Run command if the previous fails:

```bash
shadow destination_dir/ -exec rmdir subdirectory NOT mkdir subdirectory
```

Run command if the previous success:

```bash
shadow destination_dir/ -exec mkdir subdirectory AND rmdir subdirectory
```

Run command if `shadow` success or fail:

```bash
shadow destination_dir/ -exec mkdir subdirectory && rmdir destination_dir/subdirectory # If success
shadow destination_dir/ -exec rmdir subdirectory || mkdir destination_dir/subdirectory # If fails
```

Inspect if a command ran, where it runs and what it runs:

```bash
shadow --verbose destination_dir/ mkdir subdirectory1 AND mkdir subdirectory2
```

Run a demonstration:

```bash
shadow --dry destination_dir/ mkdir subdirectory1 AND mkdir subdirectory2
```

## Parallel

If you want to combine `shadow` capabilities with async, you can use it with the [GNU Parallel](https://www.gnu.org/software/parallel).

```bash
parallel shadow --verbose {} -exec rm -rf .git ::: repository1 repository2 repository3 repository4
parallel shadow --verbose {} -exec rm -rf .git :::: repository_list.txt # Describes repos like above per line
```

## License

This repository is licensed under the MIT License, allowing for extensive use, modification, copying, and distribution.
