---
title: "Introduction to Bash Scripting: Automate Linux Tasks"
date: 2026-11-26
draft: false
description: "Use variables, input, conditions, loops, and scheduled jobs to automate repeatable terminal work."
categories: ["Linux"]
tags: ["Linux"]
readingTime: true
---

## Shells and Bash

A shell is a command interpreter: it reads commands and asks the operating system to run programs. Bash (the Bourne Again Shell) is widely used on Linux and other Unix-like systems. A Bash script stores a sequence of commands in a text file so that a repeatable task can be run consistently.

## Your first script

Create a file named `hello.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

name="developer"
echo "Hello, ${name}!"
```

The first line selects Bash through the environment. The options make common scripting mistakes easier to detect: stop on failed commands, reject unset variables, and handle pipeline failures. Save the file, make it executable with `chmod +x hello.sh`, and run it with `./hello.sh`.

## Variables and user input

Assign values without spaces around `=`. Reference a variable with `$name` or `${name}`. Use `read` to collect input:

```bash
read -r -p "Enter your project name: " project
printf 'Project: %s\n' "$project"
```

Quote variable expansions in most command arguments. Quoting prevents spaces and wildcard characters in a value from being interpreted as separate words or patterns.

## Conditions and loops

An `if` statement runs commands when a test succeeds:

```bash
if [[ -d "$HOME/Documents" ]]; then
    echo "Documents directory exists"
else
    echo "Directory not found"
fi
```

A `for` loop is useful when processing a known list:

```bash
for file in *.txt; do
    [[ -e "$file" ]] || continue
    printf 'Found: %s\n' "$file"
done
```

A `while` loop repeats while its condition remains true. Use loops carefully when reading files or handling filenames; robust scripts should preserve spaces and unusual characters.

## A simple backup example

This example copies a chosen directory into a backup folder under the user's home directory. Review the source and destination before using it with important files:

```bash
#!/usr/bin/env bash
set -euo pipefail

source_dir="$HOME/Documents/project-notes"
backup_dir="$HOME/backups"
mkdir -p "$backup_dir"

if [[ -d "$source_dir" ]]; then
    cp -a "$source_dir" "$backup_dir/"
    echo "Backup copy created in $backup_dir"
else
    echo "Source directory does not exist" >&2
    exit 1
fi
```

For important data, use a tested backup tool and keep another copy separate from the machine. A script completing without an error is not the same as verifying a backup can be restored.

## Scheduling with cron

Cron can run recurring commands on a schedule. A crontab entry has five time fields followed by the command: minute, hour, day of month, month, and day of week. Use `crontab -e` to edit your personal schedule and check the command's paths and environment; cron does not necessarily use the same environment as an interactive terminal.

## Good scripting habits

- Quote variables and use `read -r` for input.
- Check that files and directories exist before using them.
- Send error messages to standard error and return non-zero status on failure.
- Prefer clear variable names and comments for non-obvious logic.
- Test with harmless sample data before running against important files.

Bash becomes valuable when small, well-tested scripts remove repetitive work from a development workflow.
