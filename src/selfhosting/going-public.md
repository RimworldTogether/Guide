# Going Public
Players are given various methods for going public when hosting the server.

## LAN (Only applies to users in the same local network. IE: same house)
* Get the `PRIVATE IP` of the machine that will be hosting the server, which can be found opening a `CMD` window and typing `ipconfig` in it.

* You will get a list of networks after that, find your current one and get the `IPv4 Address`, something along the lines of `192.168.x.x`.

* Finally, paste that IP in your server config and boot the server up, if asked by a firewall, allow it through your private network.

* After doing so, join using your `PRIVATE IP`.

## Port Forwarding (the confusing way)
* Get the PRIVATE IP of the machine that will be hosting the server, which can be found opening a CMD window and typing `ipconfig` in it.

* You will get a list of networks after that, find your current one and get the `IPv4 Address`, something along the lines of `192.168.x.x`.

* Paste that IP in your server config and boot the server up, if asked by a firewall, allow it through your public network.

* Now, you will need to port forward your server, this process varies depending on your router but a general guide can be found at [here](https://www.noip.com/support/knowledgebase/general-port-forwarding-guide).

* After doing so, launch the server back up and join using your `PUBLIC IP`, which can be found by using a webpage like [this](https://whatismyipaddress.com/).

## VPN Tunneling (the easy way)
* Download any VPN program that is broadly available (we recommend Radmin as it's the least error prone) and set up a simple network (follow the tutorial given by your chosen program).

* Have the rest of the players join your VPN network.

* Copy your VPN IP and paste it in the server config file.

* After doing so, launch the server back up and join using your VPN IP.