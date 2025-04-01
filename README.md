# remote-rsync-backup

## Overview
This script is a Bash script designed to manage backup operations using `rsync` within a Docker container. It ensures configuration management, logging, and file synchronization while implementing a locking mechanism to prevent concurrent executions.

## Features
- Uses a lock mechanism to prevent multiple instances from running simultaneously.
- Reads configuration from a predefined file.
- Ensures the existence of required directories.
- Logs all output to a dedicated log directory.
- Synchronizes files using `rsync` via a Docker container with the `alpine` image.

## Benefits of Running `rsync` Inside a Docker Container
Running the `rsync` command within a Docker container provides several benefits:
- **Environment Isolation**: The backup process is isolated from the host system, ensuring that the backup task runs in a clean, controlled environment.
- **Security**: By using a container, you ensure that the backup operations are confined within the container, preventing accidental or unauthorized modifications to the host filesystem.
- **No Risk of Removing Non-Mapped Directories**: Docker containers only have access to the volumes that are explicitly mounted, ensuring that no unintended directories or files outside of those mapped volumes will be altered or deleted. This increases stability and reliability during the backup process.
- **Stability**: Containers provide a consistent environment, which means that the backup process will work reliably across different systems, reducing the risk of configuration issues or discrepancies.
  
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
