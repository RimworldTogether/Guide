
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
