
---

# Making Your Server Public

To make your server publicly accessible, you have a few options:

## Port Forwarding (Advanced)
Port forwarding involves configuring your router to allow external access to your server. Some basic computer knowledge is helpful. Here's a [guide](https://www.noip.com/support/knowledgebase/general-port-forwarding-guide) for more help.

1. **Get Your Private IP**: Open a command prompt and type `ipconfig`. Find your current network's IPv4 Address (e.g., `192.168.x.x`).
2. **Access Router Settings**: Enter the default gateway (from `ipconfig`) into your browser to access your router’s settings.
3. **Set Up Port Forwarding**: Locate the port forwarding section (often under advanced settings). Create a new rule for the server’s port, select TCP or Both, and specify your computer. Save the rule.
4. **Join the Server**: Use your public IP (found via [WhatIsMyIPAddress](https://whatismyipaddress.com/)) to connect to the server. Ensure the server is running and the port forwarding rule is saved.

## VPN Tunneling (Simple)
VPNs like Radmin VPN make it easier to share your server with friends.

1. **Download and Install VPN**: Install a VPN program like [Radmin VPN](https://www.radmin-vpn.com/). Follow the installation instructions.
2. **Launch VPN**: Open the VPN application and activate it.
3. **Create a Network**: In the VPN app, create a new network with a unique name and strong password. Share these details with your friends.
4. **Join the Network**: Friends should install the VPN, join your network using the provided details, and get the VPN IP address.
5. **Configure the Server**: Paste the VPN IP into the server config file.
6. **Connect to the Server**: Use the VPN IP to connect to your server.

## LAN (Local Area Network)
For play within the same local network:

1. **Get Your Private IP**: Open a command prompt and type `ipconfig` to find your current network's IPv4 Address (e.g., `192.168.x.x`).
2. **Configure the Server**: Paste this private IP into your server config and start the server. Allow it through your private network firewall if prompted.
3. **Join the Server**: Use the private IP to connect to your server.

## Troubleshooting

For additional troubleshooting assistance, join our [Discord Server](https://discord.gg/NCsArSaqBW).

---

# Setting Up and Running the Server Linux Edition

Here's a step-by-step guide to set up and run your RimWorld Together server on Linux.

## Prerequisites

Ensure you have Docker installed on your Linux system. If not, follow [this guide](https://docs.docker.com/engine/install/) to install Docker.

## Download and Run the Server

Create a script with the following content to automatically download the latest release and run the server:

```bash
#!/bin/bash

# Variables
folder_name=RIM
system_type=arm # possible values are arm (for Raspberry Pi), arm64, and x64

# Get the latest release version tag
latest_tag=$(curl -sL https://api.github.com/repos/RimworldTogether/Rimworld-Together/releases/latest | grep tag_name | grep -o "[0-9\\.]*")

if [ -z "$latest_tag" ]; then
    echo "Latest tag couldn't be found"
    exit 1
fi

# Create directory and navigate into it
mkdir -p "$folder_name"
cd "$folder_name"

# Download the latest release for your system type
wget -S "https://github.com/RimworldTogether/Rimworld-Together/releases/download/$latest_tag/linux-$system_type.zip"
if [ ! -f "linux-$system_type.zip" ]; then
    echo "File couldn't be downloaded or wrong filename"
    exit 1
fi

# Unzip the downloaded file
unzip "linux-$system_type.zip"

# Start the server
echo "quit" | ./GameServer
screen -S RIMWORLD ./GameServer
```

Save this script as `start_server.sh` and make it executable:

```bash
chmod +x start_server.sh
```

Run the script to start your server:

```bash
./start_server.sh
```

## Renaming Mods

To rename mods, use the following Python script to handle invalid characters and ensure proper renaming. Place this script inside your mods folder:

```python
import os
import re
import xml.etree.ElementTree as ET

def sanitize_name(name):
    # Replace invalid characters with underscores
    return re.sub(r'[<>:"/\\|?*]', '_', name)

required_path = "Required"
for folder in os.listdir(required_path):
    folder_path = os.path.join(required_path, folder)
    if not os.path.isdir(folder_path):
        continue
    try:
        int(folder)
    except ValueError:
        continue
    about_path = os.path.join(folder_path, "About")
    if not os.path.isdir(about_path):
        continue
    xml_file = None
    if "About.xml" in os.listdir(about_path):
        xml_file = "About.xml"
    elif "about.xml" in os.listdir(about_path):
        xml_file = "about.xml"
    else:
        print(f"{folder}: About.xml not found")
        continue
    tree = ET.parse(os.path.join(about_path, xml_file))
    name = tree.getroot().find("name").text
    sanitized_name = sanitize_name(name)
    new_folder_name = f"{folder}-{sanitized_name}"
    new_folder_path = os.path.join(required_path, new_folder_name)
    os.rename(folder_path, new_folder_path)
    print(f"Renamed {folder} to {new_folder_name}")
```

Save this script as `rename_mods.py` and run it to rename your mods:

```bash
python rename_mods.py
```

By following these steps, you can set up your RimWorld Together server, make it publicly accessible, and manage your mods effectively. For any issues or further assistance, join our [Discord Server](https://discord.gg/NCsArSaqBW).
