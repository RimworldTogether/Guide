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

## Troubleshooting

For additional support or to discuss mod-related issues, join our [Discord Server](https://discord.gg/yUF2ec8Vt8).
