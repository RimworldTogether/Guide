
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

4. **Join the Server**: Use your public IP (discoverable via [WhatIsMyIPAddress](https://whatismyipaddress.com/)) to connect. Ensure the server is operational and the setup is correct.

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
3. **Join the Server**: Connect using the private IP address. Ensure your firewall settings allow traffic on the server port.

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

### Step 2: Automated Installation and Update Script

**1. Create the Startup Script**
- Create a file named `start_server.sh` with the following contents:

```bash
#!/bin/bash

# Define folder and system type
folder_name="RimWorldServer"  # Adjust folder name to preference
system_type=$(uname -m)  # Auto-detects architecture

# Define variables for clarity
supported_architectures="x64, arm (ARMv7l/ARMv6l), arm64 (Aarch64)"

# Adjust system type based on architecture
case "$system_type" in
    x86_64)
        system_type="x64"
        ;;
    armv7l|armv6l)
        system_type="arm"
        ;;
    aarch64)
        system_type="arm64"
        ;;
    *)
        echo "Unsupported architecture ($system_type)."
        echo "This script supports the following architectures: $supported_architectures."
        exit 1
        ;;
esac

# Fetch the latest release version tag from GitHub
latest_tag=$(curl -s https://api.github.com/repos/RimworldTogether/Rimworld-Together/releases/latest | grep tag_name | grep -o "[0-9\\.]*")
if [ -z "$latest_tag" ]; then
    echo "Version not found."
    exit 1
fi

# Prepare the directory and download the server
mkdir -p "$folder_name"
cd "$folder_name" || exit
wget -q "https://github.com/RimworldTogether/Rimworld-Together/releases/download/$latest_tag/linux-$system_type.zip" -O server.zip
unzip -o server.zip && rm server.zip

# Start the server in a detached screen session
screen -S "RimWorldServer" -dm bash -c './GameServer; exec bash'
echo "Server running in screen session named 'RimWorldServer'."
```

**2. Save the Script**
- Save the above script into a file named `start_server.sh` on your Linux server.

**3. Make the Script Executable**
- Change the permissions of the script to make it executable by running:

```bash
chmod +x start_server.sh
```

**4. Run the Script to Start Your Server**
- Execute the script to start your RimWorld Together server:

```bash
./start_server.sh
```

This script will:
- Automatically detect the architecture of your system and adjust the download URL accordingly.
- Fetch the latest version of the server software from the RimWorld Together GitHub repository.
- Unzip the server files into a specified directory and clean up the downloaded zip file.
- Start the server inside a `screen` session, which allows the server to run in the background and be reattached at any time.

NOTE: Make sure to also follow recommended patch note updates as this script does not automate it.

### 3. Renaming Mods with Python

Ensures proper naming conventions for mod directories:

```python
import os
import xml.etree.ElementTree as ET
import re

# Define the directory where mods are stored
required_path = "/path/to/your/mods"

def sanitize_filename(name):
    """ Sanitize filenames to remove characters that might cause issues in file systems. """
    return re.sub(r'[<>:"/\\|?*]', '_', name)

def find_correct_xml_file(files):
    """ Return 'About.xml' if present, otherwise the first XML file in the list. """
    for file in files:
        if file == "About.xml":
            return "About.xml"
    return files[0] if files else None

# Process each directory in the mods directory
for folder in os.listdir(required_path):
    folder_path = os.path.join(required_path, folder)
    
    # Ensure the folder is a directory and has a numeric ID
    if not os.path.isdir(folder_path):
        continue
    try:
        int(folder)  # This checks if the folder name is an integer (mod ID)
    except ValueError:
        continue  # Skip processing if the folder name is not an integer

    about_path = os.path.join(folder_path, "About")
    if not os.path.isdir(about_path):
        continue

    xml_files = [f for f in os.listdir(about_path) if f.lower() == 'about.xml']
    selected_xml_file = find_correct_xml_file(xml_files)

    if selected_xml_file:
        current_filepath = os.path.join(about_path, selected_xml_file)
        correct_filepath = os.path.join(about_path, "About.xml")
        
        # Rename to "About.xml" if necessary
        if selected_xml_file != "About.xml":
            os.rename(current_filepath, correct_filepath)
            print(f"Renamed {selected_xml_file} to 'About.xml' in {about_path}")

        # Parse the XML and extract the mod name
        tree = ET.parse(correct_filepath)
        mod_name = sanitize_filename(tree.getroot().find('name').text)
        new_dir_name = os.path.join(required_path, f"{folder}-{mod_name}")
        
        # Rename the directory to include the mod name
        if not os.path.exists(new_dir_name):
            os.rename(folder_path, new_dir_name)
            print(f"Renamed {folder_path} to {new_dir_name}")
    else:
        print(f"No valid 'About.xml' file found in {about_path}. Skipping directory.")
```

### Troubleshooting
Visit the [RimWorld Together Discord Server](https://discord.gg/NCsArSaqBW) for community support.
---
