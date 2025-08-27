# Run command in another location

`doppel` is a tool that allows you to run shell commands in a different location (directory).

It allows a set command to run in multiple directories and cascade depending on command failure or success.

## Features

- Run a given command in multiple directories
- Run a command if the previous fails
- Run a command if the previous success

## Installation

```bash
git clone https://codeberg.org/aocoronel/doppel
cd doppel
mv src/doppel $HOME/.local/bin
chmod +x "$HOME/.local/bin/doppel"
```

## Usage

Base usage doppel:

```bash
doppel destination_dir/ -exec mkdir subdirectory
```

Run command in multiple directories:

```bash
doppel destination_dir1/ destination_dir2/ -exec mkdir subdirectory
```

Run command if the previous fails:

```bash
doppel destination_dir/ -exec rmdir subdirectory NOT mkdir subdirectory
```

Run command if the previous success:

```bash
doppel destination_dir/ -exec mkdir subdirectory AND rmdir subdirectory
```

Run command if `doppel` success or fail:

```bash
doppel destination_dir/ -exec mkdir subdirectory && rmdir destination_dir/subdirectory # If success
doppel destination_dir/ -exec rmdir subdirectory || mkdir destination_dir/subdirectory # If fails
```

Inspect if a command ran, where it runs and what it runs:

```bash
doppel --verbose destination_dir/ mkdir subdirectory1 AND mkdir subdirectory2
```

Run a demonstration:

```bash
doppel --dry destination_dir/ mkdir subdirectory1 AND mkdir subdirectory2
```

## Parallel

If you want to combine `doppel` capabilities with async, you can use it with the [GNU Parallel](https://www.gnu.org/software/parallel) for robust async capabilities, or use builtin bash async features.

Using `doppel` with `parallel`:

```bash
parallel doppel --verbose {} -exec rm -rf .git ::: repository1 repository2 repository3 repository4
parallel doppel --verbose {} -exec rm -rf .git :::: repository_list.txt # Describes repos like above per line
```

Using `doppel` with builtin async features:

```bash
# -j JOBS
doppel -j 5 repository_list.txt -exec rm -rf .git
```

## License

This repository is licensed under the MIT License, allowing for extensive use, modification, copying, and distribution.
