# Setting up NUT for PowerWalker VI 2200RLE UPS via USB (Debian/Ubuntu)

This document outlines the steps to configure **Network UPS Tools (NUT)** to monitor a **PowerWalker VI 2200RLE** UPS connected directly via a USB cable to your Debian or Ubuntu Linux system. This setup configures NUT to run as a **netserver**, allowing other NUT clients on the network to connect and monitor the UPS.

### Prerequisites

* A Debian or Ubuntu Linux system.
* Your PowerWalker VI 2200RLE UPS connected to the Linux system via a USB cable.

---

### 1. Install NUT and Dependencies

Open a terminal and install the necessary packages:

```bash
sudo apt update
sudo apt install nut nut-client nut-server
```

### 2. Configure nut.conf

Edit the main NUT configuration file to set the operating mode:

```bash
sudo nano /etc/nut/nut.conf
```

Find the MODE line and set it to netserver:

``` apacheconf
MODE=netserver
```

Save and close the file (Ctrl+X, then Y, then Enter).
### 3. Configure ups.conf

This file defines your UPS and its connection parameters.

```bash
sudo nano /etc/nut/ups.conf
```

Add the following UPS configuration:

```
[2200R]
        driver = usbhid-ups
        port = auto
        vendorid = 0764
        productid = 0601
        desc = "PowerWalker VI 2200RLE UPS via USB"
```

Save and close the file.
### 4. Configure upsd.conf

This file configures the NUT server daemon (upsd). To allow other NUT clients on your network to connect, it needs to listen on all network interfaces (0.0.0.0) or a specific network IP address.

```bash
sudo nano /etc/nut/upsd.conf
```

Ensure the LISTEN directive is set to 0.0.0.0 to listen on all interfaces:

```
LISTEN 0.0.0.0 3493
```
> [!IMPORTANT]  
> If you set LISTEN 0.0.0.0, ensure your firewall is configured to allow inbound connections on TCP port 3493 from trusted network ranges to prevent unauthorized access.

Save and close the file.
### 5. Configure upsd.users

This file defines users that can access the NUT server. You'll need at least one user for upsmon and any potential remote clients:

```bash
sudo nano /etc/nut/upsd.users
```

Add a user (replace your_password with your desired credentials):

```
[upsmon]
    password = your_password
    upsmon master
```

Save and close the file.
### 6. Configure upsmon.conf

This file configures the NUT monitor daemon (upsmon), which monitors the UPS status and initiates actions on events like low battery:

```bash
sudo nano /etc/nut/upsmon.conf
```

Add a MONITOR line, using the UPS name 2200R (from ups.conf) and the user from upsd.users. Since upsd is running as a netserver on the same machine, upsmon will monitor localhost.

```
MONITOR 2200R@localhost 1 upsmon your_password master
```

Configure the SHUTDOWNCMD to define the command to execute during a critical UPS event (e.g., low battery):

```
SHUTDOWNCMD "/sbin/shutdown -h +0" # Graceful shutdown
```

Adjust other settings like NOTIFYFLAG, POLLINTERVAL, and TIMEOUT as needed.

Save and close the file.
### 7. Configure UDEV Rules for USB Access

For NUT to properly communicate with your USB UPS, the nut user often needs specific permissions to access the USB device. This is managed via UDEV rules. While nut packages usually install default rules, explicitly ensuring the correct rule can prevent permission issues.

First, identify the Vendor ID (idVendor) and Product ID (idProduct) of your UPS using lsusb.

```bash
lsusb
```

Look for a line similar to ID 0764:0601 PowerWalker.

Now, create or modify the UDEV rule file.

```bash
sudo nano /etc/udev/rules.d/50-ups.rules
```

Add the following line, replacing idVendor and idProduct if required.

```
SUBSYSTEM=="usb", ATTR{idVendor}=="0764", ATTR{idProduct}=="0601", MODE="0660", GROUP="nut"
```

This rule tells UDEV to set read/write permissions (MODE="0660") and assign the device to the nut group (GROUP="nut") for any USB device matching the specified Vendor ID and Product ID.

After creating/modifying the file, reload UDEV rules and trigger them:

```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

### 8. Start and Enable NUT Services

Enable and start the NUT services:

```bash
sudo systemctl enable nut-monitor.service
sudo systemctl enable nut-client.service
sudo systemctl enable nut-server.service
sudo systemctl start nut-monitor.service
sudo systemctl start nut-client.service
sudo systemctl start nut-server.service
sudo upsdrvctl stop
sudo upsdrvctl start
```

### 9. Test the Connection

Use the upsc command to check the status of your UPS:

```bash
upsc 2200R@localhost
```

If the configuration is correct, this command should display the status information from your PowerWalker VI 2200RLE UPS.
