https://chat.openai.com/c/f69136ab-215a-4875-88a6-8d5965796255

### Purpose
The main goal was to set up a home web server without a dedicated DNS or domain name. We aimed to host the server securely and simply, utilizing existing hardware and considering future plans like setting up other services like Nextcloud.

### Achievements & Wins
1. **Set Up a Local DNS Server:** We configured `dnsmasq` to handle local DNS, allowing for easy access to local resources using friendly names.
2. **Configured DHCP:** We set up DHCP to assign IP addresses dynamically and configured static leases for specific devices.
3. **Implemented Firewall Rules:** We used `ufw` to secure the server, allowing only necessary ports and disabling IPv6.
4. **Tested & Validated the Configuration:** We ensured that the setup worked across different devices, including workstations, phones, and laptops.

### Lessons Learned
- **Understanding of DNS and DHCP:** We delved into the intricacies of DNS and DHCP, learning how to configure them for a local network.
- **Firewall Configuration:** We learned how to set up `ufw` to secure the server, including allowing necessary ports for DNS and DHCP.
- **Troubleshooting Skills:** We encountered and resolved various issues, such as typos in IP addresses and conflicts with hostnames.

### Completed Tasks
- Installed and configured `dnsmasq` for DNS and DHCP.
- Set up static leases and friendly names for local resources.
- Configured `ufw` to secure the server.
- Tested the setup across different devices.

### Cleanup Tasks
- Consider monitoring the server to ensure ongoing stability.
- Regularly review and update firewall rules as needed.
- Keep the system and installed packages up to date with security patches.

### Step-by-Step Guide to Replicating the Setup

#### 1. Install dnsmasq
```bash
sudo apt-get install dnsmasq
```

#### 2. Configure dnsmasq for DNS and DHCP
Edit `/etc/dnsmasq.conf` and add the following:

```bash
# DNS Configuration
address=/www.asdf.thenest.org/192.168.0.248
address=/workhorse.asdf.thenest.org/192.168.0.237
# ... other address lines ...

# DHCP Configuration
dhcp-range=192.168.0.2,192.168.0.254,12h
dhcp-host=1C:6F:65:C5:B8:92,192.168.0.237
# ... other static leases ...

# DNS Server
dhcp-option=option:dns-server,192.168.0.248
no-resolv
```

#### 3. Install and Configure UFW
```bash
sudo apt-get install ufw
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 2222/tcp
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
sudo ufw allow 67/udp
sudo ufw allow 68/udp
# ... other rules ...
sudo ufw enable
```

#### 4. Disable IPv6 in UFW
Edit `/etc/default/ufw` and set `IPV6=no`, then reload UFW:

```bash
sudo ufw reload
```

#### 5. Test the Configuration
Ensure that local resources are accessible using friendly names and that the server is secure.

### Conclusion
We successfully set up a home web server with local DNS and DHCP, secured with a firewall. The setup is tailored to the specific needs and hardware, providing a solid foundation for future projects and services.
