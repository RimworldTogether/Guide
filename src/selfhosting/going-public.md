
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

## **Linux Server Setup and Management Guide for RimWorld Together**

### **1. Making Your Server Public**

#### **Port Forwarding (Advanced)**
Port forwarding is necessary to allow external access to your server. This method requires familiarity with Linux command line and network settings:

1. **Get Your Private IP**: Open a terminal and type `ip a` or `ifconfig` (depending on your distribution). Find the IPv4 address associated with your network interface (e.g., `eth0` or `wlan0`), which usually looks like `192.168.x.x`.
2. **Access Router Settings**: Type the default gateway address (found in the `ip a` or `ifconfig` output) into your web browser to access your router’s settings.
3. **Set Up Port Forwarding**: Navigate to the port forwarding section, often found under "Advanced Settings" in the router's interface. Create a new rule for TCP port 25555 (or another port if your setup differs), assigning it to the private IP address noted earlier. Save or apply the changes.
4. **Configure Firewall on Linux**: Ensure your Linux firewall allows traffic on the server port:
   ```bash
   sudo iptables -A INPUT -p tcp --dport 25555 -j ACCEPT  # For iptables
   sudo ufw allow 25555/tcp                              # For UFW (Uncomplicated Firewall)
   ```
5. **Join the Server**: Connect to your server using the public IP address (find your public IP via websites like [WhatIsMyIPAddress](https://whatismyipaddress.com/)). Ensure your server is operational and that the port forwarding is correctly set up.

### **2. Automated Installation and Update Script**

This Bash script automates downloading, updating, and running the latest release of the RimWorld Together server on Linux:

```bash
#!/bin/bash

# Define variables
folder_name="RimWorldServer"
system_type=$(uname -m)  # Dynamically determines the system architecture

# Adjust system type for the download URL
case "$system_type" in
    x86_64) system_type="x64" ;;
    arm*) system_type="arm" ;;
    aarch64) system_type="arm64" ;;
    *) echo "Unsupported architecture"; exit 1 ;;
esac

# Fetch the latest release version tag from GitHub
latest_tag=$(curl -sL https://api.github.com/repos/RimworldTogether/Rimworld-Together/releases/latest | grep tag_name | grep -o "[0-9\\.]*")

if [ -z "$latest_tag" ]; then
    echo "Failed to find the latest version."
    exit 1
fi

# Prepare the installation directory
mkdir -p "$folder_name"
cd "$folder_name" || exit

# Download and unzip the latest version
wget "https://github.com/RimworldTogether/Rimworld-Together/releases/download/$latest_tag/linux-$system_type.zip"
unzip -o "linux-$system_type.zip"

# Start the server in a new screen session
screen -S "RimWorldServer" -dm bash -c './GameServer; exec bash'
echo "Server started in a detached screen session."
```

### **3. Renaming Mods with Python**

This Python script renames mod directories to their in-game names, helping manage mod files effectively:

```python
import os
import xml.etree.ElementTree as ET
import re

def sanitize_filename(name):
    return re.sub(r'[<>:"/\\|?*]', '_', name)

mod_directory = "/path/to/your/mods"

for mod in os.listdir(mod_directory):
    about_path = os.path.join(mod_directory, mod, "About")
    if not os.path.isdir(about_path):
        continue
    xml_file = next((f for f in os.listdir(about_path) if f.lower() == "about.xml"), None)
    if xml_file:
        tree = ET.parse(os.path.join(about_path, xml_file))
        mod_name = sanitize_filename(tree.getroot().find('name').text)
        new_dir_name = os.path.join(mod_directory, f"{mod}-{mod_name}")
        os.rename(os.path.join(mod_directory, mod), new_dir_name)
        print(f"Renamed {mod} to {new_dir_name}")
```

### **Troubleshooting**

For additional help or community support, join the [RimWorld Together Discord Server](https://discord.gg/NCsArSaqBW).

---
