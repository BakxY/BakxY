# Reset Wi-Fi config of a Remarkable 2

I had some issues when I pressed forget for a network on my remarkable and the tried to reconnect to this network with a new password. I thought that I could reset the entire network config of my remarkable to fix this. So here is how to do it:

1. Connect to your remarkable with SSH ([Tutorial](https://remarkable.guide/guide/access/ssh.html))
2. Open the `/home/root/.config/remarkable/xochitl.conf` file using nano (`nano /home/root/.config/remarkable/xochitl.conf`)
3. Go to the part of this file that starts with `[wifinetworks]`
4. Delete every line coming after that (Lines start with the SSID of a network)
5. Save the file
6. Restart the remarkable system with `systemctl restart xochitl`
