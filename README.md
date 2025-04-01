# remote-rsync-backup

## Overview

`remote-rsync-backup` is a Bash script designed to facilitate automated backup operations using `rsync` within a Docker container. The script provides a robust solution for syncing files with remote servers, ensuring security, stability, and consistency throughout the backup process. It leverages Docker for environment isolation and implements a locking mechanism to prevent concurrent execution.

## Features

- **Locking Mechanism**: Ensures that only one instance of the script runs at any given time.
- **Configuration Management**: Reads configurations from predefined files for different servers.
- **Logging**: Outputs all logs to dedicated files for both standard output and errors.
- **File Synchronization**: Uses `rsync` for reliable file transfer and synchronization inside a Docker container.
- **Docker Container**: Runs `rsync` within an isolated container (`alpine`), ensuring a clean environment for each backup.

## Benefits of Running `rsync` Inside a Docker Container

Running `rsync` within a Docker container offers several advantages:

- **Isolation**: The backup process is isolated from the host system, which helps ensure it runs in a clean and controlled environment, avoiding conflicts with host system configurations.
- **Security**: Docker containers restrict the backup process to only the resources they are granted access to, reducing the risk of accidental modifications to the host filesystem.
- **Stability**: Containers provide a consistent environment, meaning the backup operation will behave the same way across different systems, reducing discrepancies or failures due to environmental differences.
- **No Risk of Unintended Deletion**: Docker ensures that the backup process only interacts with mapped volumes, minimizing the risk of deleting or modifying directories outside the backup scope.

## Prerequisites

- Bash shell
- Docker installed and running
- Proper configuration files in place
- SSH access to remote servers

## Installation

1. Clone or copy the script to your desired location.
2. Ensure all configuration files are set up correctly.
3. Make sure Docker is installed on the machine.

## Usage

To run the backup process for a specific server, use the following command:

```bash
./script/remote-rsync-backup <server_name>
```
Where `<server_name>` corresponds to the configuration file for the desired server.

## Configuration

The script uses several configuration files:

- `conf/<script_name>`: The main configuration file containing script-specific settings.
- `log/`: Directory where log files are stored.
- `SERVERSCONFDIR/<server_name>/include`: List of files and directories to be backed up.
- `SERVERSCONFDIR/<server_name>/exclude`: (Optional) List of files and directories to exclude from the backup.

## Locking Mechanism

The script uses a lock file located at `$TEMP/<script_name>.lock`. This ensures that only one instance of the script runs at a time, preventing issues related to simultaneous execution.

## Logging

Logs are saved in the `log/` directory:

- Standard output is stored in `log/<script_name>.out`.
- Errors are stored in `log/<script_name>.err`.

## Example Run

```bash
./remote-rsync-backup myserver
```
This will begin the backup process for the server named `myserver`, using its specific configuration.

## Exit Codes

- `0`: Success
- `1`: Configuration or directory issue
- `3`: Lock acquisition failure

## Scheduling with Cron

You can schedule the backup process to run automatically using cron. For example, to run a backup every 24 hours at 1:00 AM, add the following entry to your crontab:

```bash
# Run a cron job every 24 hours at 1:00 AM
01 01 * * * for SERVER in `ls /nas/backup/server/`; do /nas/script/remote-rsync-backup $SERVER; done
```

This cron job will loop through each server directory in `/nas/backup/server/` and execute the backup script for each one at the scheduled time.

## Notes

- Ensure that `DSTPATH` is set correctly and points to the desired backup destination.
- The script executes `rsync` inside a Docker container using the `alpine` image.
- Run the script with appropriate permissions to access the required files and directories.
