# Setting up NUT for PowerWalker UPS with PowerMaster+ Local over SNMP (Debian/Ubuntu)

This document outlines the steps to configure Network UPS Tools (NUT) to monitor a PowerWalker UPS using the SNMP functionality provided by the PowerMaster+ Local software on Debian or Ubuntu Linux. This assumes you have PowerPaster+ setup with SNMP enabled and its IP address and SNMP community string are known.

**Prerequisites:**

* A Debian or Ubuntu Linux system.
* Network connectivity between the NUT machine and the PowerWalker UPS.
* The IP address of your PowerWalker UPS.
* The SNMP read community string configured on your PowerWalker UPS (default is often `public`).

**Steps:**

1.  **Install NUT and the SNMP Driver:**

    Open a terminal and install the necessary packages:

    ```bash
    sudo apt update
    sudo apt install nut nut-client nut-snmp
    ```

2.  **Configure `nut.conf`:**

    Edit the main NUT configuration file to set the operating mode:

    ```bash
    sudo nano /etc/nut/nut.conf
    ```

    Find the `MODE` line and set it to `standalone`:

    ```
    MODE=standalone
    ```

    Save and close the file (Ctrl+X, then Y, then Enter).

3.  **Configure `ups.conf`:**

    This file defines your UPS and its connection parameters:

    ```bash
    sudo nano /etc/nut/ups.conf
    ```

    Add a new UPS config, replacing the placeholder values with your actual UPS information:

    ```
    [powerwalker_snmp]
        driver = snmp-ups
        port = <IP_ADDRESS_OF_POWERWALKER_UPS>
        community = <SNMP_READ_COMMUNITY>
        mibs = ietf 
        desc = "PowerWalker UPS via SNMP"
    ```

    * Replace `<IP_ADDRESS_OF_POWERWALKER_UPS>` with the actual IP address of your PowerWalker UPS.
    * Replace `<SNMP_READ_COMMUNITY>` with the SNMP read community string (e.g., `public`).

    Save and close the file.

4.  **Configure `upsd.conf`:**

    This file configures the NUT server daemon (`upsd`). Ensure it's listening for local connections so `upsmon` can communicate with it:

    ```bash
    sudo nano /etc/nut/upsd.conf
    ```

    Ensure the `LISTEN` directive includes `127.0.0.1`:

    ```
    LISTEN 127.0.0.1 3493
    # You might have other LISTEN lines, that's okay
    ```

    Save and close the file.

5.  **Configure `upsd.users`:**

    This file defines users that can access the NUT server. You'll need at least one user for `upsmon`:

    ```bash
    sudo nano /etc/nut/upsd.users
    ```

    Add a user (replace `your_password` with your desired credentials):

    ```
    [upsmon]
        password = your_password
        upsmon master
    ```

    Save and close the file.

6.  **Configure `upsmon.conf`:**

    This file configures the NUT monitor daemon (`upsmon`), which monitors the UPS status and initiates actions on events like low battery:

    ```bash
    sudo nano /etc/nut/upsmon.conf
    ```

    Add a `MONITOR` line, using the UPS name defined in `ups.conf` and the user from `upsd.users`:

    ```
    MONITOR powerwalker_snmp@localhost 1 upsmon your_password master
    ```

    Configure the `SHUTDOWNCMD` to define the command to execute during a critical UPS event (e.g., low battery):

    ```
    SHUTDOWNCMD "/sbin/shutdown -h +0" # Graceful shutdown
    ```

    Adjust other settings like `NOTIFYFLAG`, `POLLINTERVAL`, and `TIMEOUT` as needed.

    Save and close the file.

7.  **Start and Enable NUT Services:**

    Enable and start the NUT services:

    ```bash
    sudo systemctl enable nut-server.service
    sudo systemctl enable nut-monitor.service
    sudo systemctl start nut-server.service
    sudo systemctl start nut-monitor.service
    ```

8.  **Test the Connection:**

    Use the `upsc` command to check the status of your UPS:

    ```bash
    upsc powerwalker_snmp@localhost
    ```

    If the configuration is correct, this command should display the status information from your PowerWalker UPS retrieved via SNMP.

**Troubleshooting (Debian/Ubuntu):**

* **"Error: Driver not connected"**: Ensure the `nut-snmp` package is installed and the driver name in `/etc/nut/ups.conf` is exactly `snmp-ups`. Double-check the IP address and community string. Verify network connectivity using `ping <IP_ADDRESS_OF_POWERWALKER_UPS>`.
* **Check NUT Logs:** Look for error messages using `journalctl -u nut-server.service` and `journalctl -u nut-monitor.service`.
* **SNMP Availability:** Confirm that the SNMP service is enabled and running in PowerMaster+ Local. You can test basic SNMP communication from your Debian/Ubuntu machine using:
    ```bash
    sudo apt install snmp
    snmpwalk -v 2c -c <SNMP_READ_COMMUNITY> <IP_ADDRESS_OF_POWERWALKER_UPS>
    ```
