## Issue

I wasn't able to connect to my esxi management trough my vpn. This management ui is hosted on port 443 on the ip 192.168.30.130 (in Lan network).

From where can I access the ui:

| Device     | Network | IP             | Access (y/n) | Error code |
|:-----------|:--------|:---------------|--------------|------------|
| Main PC    | LAN     | 192.168.30.21  | y            | -          |
| VPN Server | DMZ     | 192.168.50.134 | n            | timeout    |
| VPN Client | VPN DMZ | 10.0.8.2       | n            | timeout    |

## Solution
