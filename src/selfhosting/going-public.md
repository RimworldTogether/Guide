
---

# Making Your Server Public

To make your server publicly accessible, you have several options depending on your setup:

## Port Forwarding (Advanced)
Port forwarding enables external access to your server. Here's how to configure it:

1. **Get Your Private IP**:
   - **Windows**: Open Command Prompt and type `ipconfig`.
   - **Linux**: Open Terminal and type `ip a` or `ifconfig`.
   Identify your network's IPv4 Address (e.g., `192.168.x.x`).

2. **Access Router Settings**: Enter your default gateway IP into your browser to access your router's settings.

3. **Set Up Port Forwarding**: Navigate to the port forwarding section, typically found under "Advanced". Create a rule for TCP port 25555, assigning it to your IPv4 address, and save the configuration.

4. **Enable Device Communication**: In your router settings, ensure that devices on your network are allowed to communicate with each other. This is often found under settings like "LAN Isolation" or "Device Communication".

5. **Join the Server**: Use your public IP (discoverable via [WhatIsMyIPAddress](https://whatismyipaddress.com/)) to connect. Ensure the server is operational and the setup is correct.

## VPN Tunneling (Windows Only)
Use a VPN to simplify server sharing without complex router configurations:

1. **Download and Install VPN**: We recommend using [Radmin VPN](https://www.radmin-vpn.com/).
2. **Launch VPN**: Start the VPN service and create a network with a unique name and strong password.
3. **Invite Friends**: Share the network details with your friends so they can join.
4. **Connect to the Server**: Use the VPN-generated IP to establish a connection.

## LAN (Local Area Network)
For local network connections without internet:

1. **Get Your Private IP**: Retrieve your private IP using `ipconfig` (Windows) or `ip a` (Linux).
2. **Configure the Server**: Input the private IP into your server's configuration.
3. **Enable Device Communication**: In your router settings, ensure that devices on your network are allowed to communicate with each other.
4. **Join the Server**: Connect using the private IP address. Ensure your firewall settings allow traffic on the server port.

## Troubleshooting
For additional support, join our [Discord Server](https://discord.gg/NCsArSaqBW).

---

## Linux Server Setup and Management Guide for RimWorld Together

### 1. Making Your Server Public

#### Port Forwarding (Advanced)
This process is essential for allowing external server access, particularly for those hosting on their own Linux machines:

1. **Get Your Private IP**: Open a terminal and type `ip a` or `ifconfig`. Locate the IPv4 address linked to your network interface (e.g., `eth0` or `wlan0`).
2. **Access Router Settings**: Enter the default gateway shown in your command output into a browser to modify your router’s settings.
3. **Set Up Port Forwarding**: In the router settings under "Advanced Settings", establish a new rule for TCP port 25555 linked to your noted private IP address. Confirm the changes.
4. **Configure Firewall on Linux**: Depending on which firewall management tool your distribution uses, apply the corresponding command:
   ```bash
   sudo iptables -A INPUT -p tcp --dport 25555 -j ACCEPT  # iptables
   sudo ufw allow 25555/tcp                              # UFW
   sudo firewall-cmd --add-port=25555/tcp --permanent    # firewalld
   sudo firewall-cmd --reload
   ```
5. **Join the Server**: Use your public IP to connect, ensuring everything is configured properly.

---

### Step 2: Automated Installation and Update Script

**1. Create the Startup Script**
- Create a file named `start_server.sh` with the following contents:

```bash
#!/bin/bash

# Configuration
folder_name="RimWorldServer"  # Adjust folder name as per your preference
system_type=$(uname -m)  # Auto-detect system architecture
use_screen=true  # Set to false if you do not want to use screen
auto_update=false  # Set to true if you want to enable automatic updates
force_old_start=false  # Set to true to skip update prompts and start the old version
version_file="version.txt"  # File to store the current version

echo "Starting RimWorld Together server setup..."

# Check for necessary tools
for tool in curl unzip; do
    if ! command -v $tool &>/dev/null; then
        echo "Error: $tool is required but not installed."
        exit 1
    fi
done

if [ "$use_screen" = true ] && ! command -v screen &>/dev/null; then
    echo "Error: screen is required but not installed. Set use_screen to false if you do not want to use it."
    exit 1
fi

# Detect system architecture
case "$system_type" in
    x86_64) system_type="x64" ;;
    armv7l|armv6l) system_type="arm" ;;
    aarch64) system_type="arm64" ;;
    *) echo "Error: Unsupported architecture ($system_type). Supported: x64, arm, arm64."; exit 1 ;;
esac
echo "Detected system architecture: $system_type"

# Fetch the latest version tag from GitHub
echo "Fetching the latest version tag from GitHub..."
latest_tag=$(curl -sL https://api.github.com/repos/RimworldTogether/Rimworld-Together/releases/latest | grep tag_name | grep -o "[0-9\\.]*")
if [ -z "$latest_tag" ]; then
    echo "Error: Could not fetch the latest version tag from GitHub."
    exit 1
fi
echo "Latest version tag is $latest_tag."

# Check if the version is up to date
if [ -f "$folder_name/$version_file" ] && grep -q "$latest_tag" "$folder_name/$version_file"; then
    echo "GameServer is up to date (version $latest_tag)."
else
    echo "New version $latest_tag available."
    if [ "$auto_update" = true ]; then
        update=true
    elif [ "$force_old_start" = true ]; then
        update=false
        echo "Manual update required. Continuing with the old version."
        sleep 5  # Pause to ensure the message is seen
    else
        sleep 5  # Pause to ensure the message is seen
        read -p "Automatic update is disabled. Do you want to update to the latest version? (yes/no): " update_choice
        case "$update_choice" in
            y|Y|yes|Yes) update=true ;;
            n|N|no|No) update=false ;;
            *) echo "Invalid choice. Exiting."; exit 1 ;;
        esac
    fi

    if [ "$update" = true ]; then
        echo "Updating GameServer to version $latest_tag..."

        mkdir -p "$folder_name"
        cd "$folder_name" || { echo "Error: Could not change to directory $folder_name"; exit 1; }

        echo "Downloading server files..."
        curl -L "https://github.com/RimworldTogether/Rimworld-Together/releases/download/$latest_tag/linux-$system_type.zip" -o server.zip
        if [ $? -ne 0 ]; then
            echo "Error: Download failed."
            exit 1
        fi

        echo "Unzipping server files..."
        unzip -o server.zip && rm server.zip
        if [ $? -ne 0 ]; then
            echo "Error: Unzipping the server files failed."
            exit 1
        fi

        echo "$latest_tag" > "$version_file"
        echo "GameServer updated to version $latest_tag."
    else
        echo "Continuing with the current version."
    fi
fi

# Start the server
if [ "$use_screen" = true ]; then
    echo "Starting the server in a detached screen session..."
    sleep 5  # Pause to ensure the message is seen
    if ! screen -dmS "RimWorldServer" bash -c './GameServer; exec bash'; then
        echo "Error: Failed to start the server in a screen session."
        exit 1
    fi
    echo "Server is running in a screen session named 'RimWorldServer'. You can attach to the session using 'screen -r RimWorldServer'."
else
    echo "Starting the server directly without screen."
    sleep 5  # Pause to ensure the message is seen
    ./GameServer &
    server_pid=$!
    echo "Server is now running directly (PID: $server_pid)."
fi
```

### Key Updates:
1. **Sleep Before User Choices**: Added `sleep` commands before user prompts to ensure the messages are seen (approximately 5 seconds).
2. **Helpful Echoes**: Ensured all important echoes are seen before the server starts sending messages.

### Usage:
- **To enable automatic updates**, set `auto_update` to `true`.
- **To force start the old version without prompting for an update**, set `force_old_start` to `true`.
- **To run the script** and check for updates or start the server, use the following commands:
  ```bash
  chmod +x start_server.sh
  ./start_server.sh
  ```

### Save the Script
- Save the script into a file named `start

_server.sh` on your Linux server.

### Make the Script Executable
- Change the permissions of the script to make it executable by running:
  ```bash
  chmod +x start_server.sh
  ```

### Run the Script to Start Your Server
- Execute the script to start your RimWorld Together server:
  ```bash
  ./start_server.sh
  ```

This script will:
- Automatically detect the architecture of your system and adjust the download URL accordingly.
- Fetch the latest version of the server software from the RimWorld Together GitHub repository.
- Unzip the server files into a specified directory and clean up the downloaded zip file.
- Start the server inside a `screen` session, allowing the server to run in the background and be reattached at any time.

**NOTE:** Follow recommended patch note updates as this script does not automate them.

---

# Additional Notes

Please head over to the [Mods section](https://rimworldtogether.github.io/Guide/selfhosting/mods.html) of the guide to finish setting up your server's mods

## Troubleshooting

For additional support or to discuss mod-related issues, join our [Discord Server](https://discord.gg/NCsArSaqBW).

---
