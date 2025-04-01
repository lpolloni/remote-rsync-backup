# remote-rsync-backup

## Overview
This script is a Bash script designed to manage backup operations using `rsync` within a Docker container. It ensures configuration management, logging, and file synchronization while implementing a locking mechanism to prevent concurrent executions.

## Features
- Uses a lock mechanism to prevent multiple instances from running simultaneously.
- Reads configuration from a predefined file.
- Ensures the existence of required directories.
- Logs all output to a dedicated log directory.
- Synchronizes files using `rsync` via a Docker container with `alpine`.

## Prerequisites
- Bash shell
- Docker installed and running
- Configuration files properly set up
- SSH access to remote servers

## Installation
1. Clone or copy the script to your desired location.
2. Ensure the required configuration files are in place.
3. Install Docker if not already installed.

## Usage
Run the script with the required parameters:
```bash
./remote-rsync-backup <server_name>
```
Where `<server_name>` corresponds to the server whose configuration will be used.

## Configuration
The script relies on the following configuration files:
- `conf/<script_name>`: Main configuration file.
- `log/`: Directory for storing logs.
- `SERVERSCONFDIR/<server_name>/include`: List of files/directories to be backed up.
- `SERVERSCONFDIR/<server_name>/exclude`: (Optional) List of files/directories to be excluded from the backup.

## Locking Mechanism
To avoid simultaneous executions, the script uses a lock file stored in `$TEMP/<script_name>.lock`. If another instance is running, it will either wait (if configured) or exit.

## Logging
All logs are stored in the `log/` directory. 
- Standard output is logged in `log/<script_name>.out`
- Errors are logged in `log/<script_name>.err`

## Example Run
```bash
./remote-rsync-backup myserver
```
This will start the backup process for `myserver`, using its corresponding configuration.

## Exit Codes
- `0`: Success
- `1`: Configuration or directory issue
- `3`: Lock acquisition failure

## Notes
- Ensure that the `DSTPATH` is properly set and matches the expected backup directory.
- The script runs `rsync` within a Docker container using the `alpine` image.
- The script must be executed with appropriate permissions to access required directories and files.
