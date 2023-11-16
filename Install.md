# pfSense Virtual Lab Installation Guide

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
- [Post-Installation Configuration](#post-installation-configuration)
- [Firewall Configuration](#firewall-configuration)
- [Advanced Configuration: Isolating the Network](#advanced-configuration-isolating-the-network)
- [Conclusion](#conclusion)

## Introduction
This guide provides step-by-step instructions on how to install pfSense in VirtualBox for setting up a virtual lab environment.

## Prerequisites
- Oracle VM VirtualBox installed on your machine
- pfSense ISO file downloaded from the official pfSense website
- Windows Server 2022 or another VM

## Installation Steps

### Step 1: Welcome Screen
![pfSense Welcome Screen](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/1.png)

![pfSense Welcome Screen](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/2.png)
1. Open VirtualBox and start the pfSense virtual machine.
2. Select `Install pfSense` and press Enter to begin the installation process.

### Step 2: Selecting the Installation Type
![Installation Type](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/3.png)
1. Choose the `Auto (ZFS)` option for a guided root-on-ZFS setup which is recommended for new installations.
2. Press Enter to proceed with the ZFS configuration.

### Step 3: ZFS Configuration
![ZFS Configuration](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/5.png)
1. In the ZFS Configuration menu, you can select the type of Virtual Device you want to use:
   - `stripe` - No redundancy
   - `mirror` - Mirror (n-way mirroring)
   - `raidz` variations for different levels of redundancy
2. For a lab environment, `stripe` may be sufficient. Use the arrow keys to select your choice and press Enter.

### Step 4: Disk Selection
![Disk Selection](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/6.png)
1. Select the disk where pfSense will be installed. Typically, this will be `ada0` for VirtualBox.
2. Press Enter to confirm your selection.

### Step 5: Final Configuration Options
![Final Configuration Options](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/4.png)
1. Before the installation starts, you can rescan devices, check disk information, set pool name, and other options.
2. After configuring options as necessary, navigate to `Install` and press Enter to start the installation.

## Post-Installation Configuration


![Lan interface](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/7.png)

### Step 6: Configuring the LAN Interface

After the installation, you will configure your network interfaces. The LAN interface is an essential part of your network setup as it connects your internal network to pfSense.

1. Connect to the pfSense console (either via direct access to the machine or through VirtualBox's console access).
2. Select the option to `Set interface(s) IP address`.
3. Choose the LAN interface (typically `em1` or `re1` depending on your virtual machine's network adapter).
4. Enter the desired IP address for the LAN interface: `192.168.2.1`.
5. Specify the subnet mask. For most small networks, `255.255.255.0` is typical, which represents a `/24` network.
6. Decide whether you want to enable the DHCP server on the LAN interface. If yes, define the range of IP addresses that the DHCP server will offer to clients.
7. Save the changes and proceed with the configuration.

### Step 7: Accessing the pfSense Web Interface
The pfSense web interface is the primary method for managing your pfSense firewall.

1. On a separate computer that is connected to the same network as the pfSense LAN interface, open a web browser.
2. Enter the IP address of the pfSense LAN interface into the browser’s address bar: `http://192.168.2.1`.
3. You should see the pfSense login page. The default username is `admin` and the default password is `pfsense`.
4. After logging in, you'll be prompted to change the default password. It is highly recommended to do so for security purposes.
5. Once logged in, the web interface provides a dashboard that gives you a quick overview of your system status, including interfaces, firewall rules, and services running on your firewall.

## Firewall Configuration

### Step 8: Running the Setup Wizard
![pfSense Setup Wizard](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/10.png)
1. Upon logging in, you will be greeted with the Setup Wizard.
2. Follow the wizard to configure basic settings for your pfSense installation.

### Step 9: Setting Firewall Rules
![Firewall Rules](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/12.png)
1. Navigate to `Firewall` > `Rules` to set up your firewall rules.
2. Here you can add, modify, and delete rules that control traffic flow through your network.

### Step 10: Adding Firewall Rules
![Add Firewall Rule](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/11.png)
1. To create a new rule, click on `Add` and configure the rule settings such as action, interface, protocol, source, and destination.
2. For example, to block all traffic except a specific site, set the action to 'Block' and specify the allowed destination IP or URL.

### Step 11: System Information and Status
![System Information](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/10.png)
1. You can view your system information by navigating to `Status` > `System`.
2. This page provides details about your pfSense installation, such as version, uptime, and CPU type.

### Step 12: Configuring Client IP Settings
![Client IP Configuration](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/8.png)
1. On a client machine, configure the IP settings to match the pfSense network. For instance:
   - IP address: `192.168.2.2`
   - Subnet mask: `255.255.255.0`
   - Default gateway: `192.168.2.1`
2. Set the DNS server addresses to use DNS servers of your choice.

### Step 13: Verifying Internet Access
![Internet Access Verification](https://github.com/Skunsara/Pfsense-Configuration/blob/main/Screenshots/13.png)
1. After configuring the network settings, open a web browser and try accessing a website to verify internet connectivity.
2. If the site loads successfully, the pfSense firewall is routing and filtering traffic as expected.

## Advanced Configuration: Isolating the Network

### Step 14: Creating Isolation Rules
To restrict network access and only allow navigation to `https://www.welcometothejungle.com`, you need to create specific firewall rules.

1. Navigate to `Firewall` > `Rules` > `LAN`.
2. Create a new rule by clicking on the `Add` button located at the bottom or top of the list.

#### Setting Up the Allow Rule
3. Set the `Action` to `Pass` to allow traffic.
4. Under `Interface`, ensure `LAN` is selected.
5. Set the `Address Family` to `IPv4`.
6. For `Protocol`, select `TCP`.
7. Under the `Source`, choose `any` to apply the rule to all sources.
8. For the `Destination`, you need to specify the IP address of `www.welcometothejungle.com`. Since firewall rules do not accept domain names, you'll need to resolve the domain to its IP address in advance.
9. Set the `Destination Port Range` to `HTTP/HTTPS` (ports 80 and 443) as web traffic typically uses these ports.
10. Provide a description for the rule, such as "Allow access to Welcome to the Jungle".
11. Click `Save` and then `Apply Changes`.

#### Setting Up the Block Rule
12. Now, create a second rule to block all other traffic.
13. Click on `Add` to create a new rule.
14. Set the `Action` to `Block`.
15. Repeat steps 4 through 7 to match the 'Allow' rule.
16. Set the `Destination` to `any` to apply the rule to all destinations.
17. Leave the `Destination Port Range` as `any`.
18. Provide a description for the rule, like "Block all other sites".
19. Click `Save` and then `Apply Changes`.

### Step 15: Verifying the Rules
After setting up your rules:

1. Move the `Allow` rule above the `Block` rule. pfSense processes firewall rules top to bottom and stops at the first match. If the `Block` rule is first, it will block all traffic, including the site you want to allow.
2. Test the rules by attempting to navigate to `https://www.welcometothejungle.com` and then to any other website. Only the specified site should be accessible.

### Step 16: Troubleshooting
If you encounter issues:

1. Check the firewall logs under `Status` > `System Logs` > `Firewall` to see if traffic is being blocked or allowed as expected.
2. Ensure the IP address for `www.welcometothejungle.com` hasn't changed. If the site uses multiple IP addresses or a content delivery network, you may need to allow a range of IPs or update the rule accordingly.
3. Confirm that the rules are in the correct order and that there are no conflicting rules above them.


## Conclusion
With the completion of these steps, your pfSense firewall is now configured to route and filter traffic in your virtual lab environment. You have also learned how to restrict access to a single website, which can be useful for various focused networking scenarios. As you continue to work with pfSense, you can explore more advanced features and configurations to suit your specific networking needs.
