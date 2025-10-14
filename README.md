# OpenRGB Daemon

A systemd service that automatically starts the OpenRGB server on system boot.

## Overview

This project provides a systemd service file that launches OpenRGB in server mode at system startup. This allows for automated RGB lighting control and remote management of RGB devices. The project is intended for use on the Tenstorrent QuietBox Global Edition (QBGE) and includes an OpenRGB profile to set the lights on that device to our preferred startup mode. However, you should be able to install the project and then use an OpenRGB client to control the lights on any devices. Note that using the project outside of our intended use case is not officially supported and we will not be able to provide help or support.

## Prerequisites

- OpenRGB installed on your system (available at `/usr/bin/openrgb`)

## Installation

### Using the Tenstorrent PPA (Recommended)

Add the Tenstorrent PPA and install the package:

```bash
# Add the Tenstorrent PPA
echo "deb [signed-by=/etc/apt/keyrings/tt-pkg-key.asc] https://ppa.tenstorrent.com/ubuntu/ $( cat /etc/os-release | grep "^VERSION_CODENAME=" | sed 's/^VERSION_CODENAME=//' ) main" | sudo tee /etc/apt/sources.list.d/tenstorrent.list > /dev/null

sudo mkdir -p /etc/apt/keyrings; sudo chmod 755 /etc/apt/keyrings

sudo wget -O /etc/apt/keyrings/tt-pkg-key.asc https://ppa.tenstorrent.com/ubuntu/tt-pkg-key.asc 

# Install the package
sudo apt update
sudo apt install tt-openrgb-daemon

# Enable and start the service
sudo systemctl enable tt-openrgb-daemon.service
sudo systemctl start tt-openrgb-daemon.service
```

The PPA is available at [ppa.tenstorrent.com](https://ppa.tenstorrent.com).

### Using the Debian Package Directly

Download the latest `.deb` file from the [GitHub Releases](https://github.com/tenstorrent/openrgb-daemon/releases) page and install it:

```bash
sudo dpkg -i tt-openrgb-daemon_*.deb
sudo systemctl daemon-reload
sudo systemctl enable tt-openrgb-daemon.service
sudo systemctl start tt-openrgb-daemon.service
```

### Manual Installation

1. Copy the service file to the systemd directory:
   ```bash
   sudo cp tt-openrgb-daemon.service /etc/systemd/system/
   ```

2. Reload systemd to recognize the new service:
   ```bash
   sudo systemctl daemon-reload
   ```

3. Enable the service to start on boot:
   ```bash
   sudo systemctl enable tt-openrgb-daemon.service
   ```

4. Start the service immediately (optional):
   ```bash
   sudo systemctl start tt-openrgb-daemon.service
   ```

## Usage

Once installed and enabled, the OpenRGB server will automatically start on system boot and be available on the default port (6742).

### Managing the Service

- Check service status:
  ```bash
  sudo systemctl status tt-openrgb-daemon.service
  ```

- Stop the service:
  ```bash
  sudo systemctl stop tt-openrgb-daemon.service
  ```

- Restart the service:
  ```bash
  sudo systemctl restart tt-openrgb-daemon.service
  ```

- View service logs:
  ```bash
  journalctl -u tt-openrgb-daemon.service
  ```

## Applying OpenRGB Profiles

To automatically apply a specific RGB pattern on startup, you can create an OpenRGB profile (.orp file) and configure it to load when the service starts.

## License

This project is licensed under the Apache License 2.0. See the LICENSE file for details.

## About

Created by Tenstorrent.
