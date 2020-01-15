This guide has been tested on the following setup:
1. Raspberry Pi 3
2. [Kali linux image for Raspberry Pi](https://images.offensive-security.com/arm-images/kali-linux-2019.4-rpi.img.xz)
3. [Canakit wifi dongle](https://www.amazon.com/CanaKit-Raspberry-Wireless-Adapter-Dongle/dp/B00GFAN498) 

Once you have installed Kali linux, ensure that the Wifi Dongle isn't plugged in. Since there is an onboard wifi chip on the RPi3, there might be a confusion in the order of `id` assignment and `wlan0` and `wlan1` might get interchanged.

Ensure that inbuilt wifi is wlan0 by running `ifconfig`. There should be only one wireless interface (because you have unplugged the dongle) called `wlan0`

1. We will host the access point using wlan1 (the Wifi Dongle)

```bash
cat > /etc/network/interfaces << EOF
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet dhcp
auto wlan1
iface wlan1 inet static
address 192.168.0.1/24
gateway 192.168.0.1
EOF
```

2. DHCP installation

```bash
apt update   # This will say that there are packages available for upgrade and blah blah.. ignore all of that.
apt install dhcpd -y  # We're actually interested in udhcpd, which is what will get installed using this command anyway.
```

Modify `/etc/udhcpd.conf`
```bash
start           192.168.0.100
end             192.168.0.154
interface       wlan1
# Optional
opt     dns     1.1.1.1
option  subnet  255.255.255.0
opt     router  192.168.0.1
option  domain  local
```

Restart the service:

```bash
service udhcpd restart
```


3. HostAPd installation
This package enables the host to act as a Wifi Access Point.

```bash
apt install hostapd -y
```

If it doesn't exist, create the following file and make the appropriate entries:
```bash
cat > /etc/hostapd/hostapd.conf <<EOF
# This is the name of the WiFi interface we configured above
interface=wlan1
# Use the nl80211 driver
driver=nl80211
# This is the name of the network
ssid=RpiEK
# Use the 2.4GHz band
hw_mode=g
# Use channel 6
channel=6
# Enable WMM
wmm_enabled=1
# Accept all MAC addresses
macaddr_acl=0
# Use WPA authentication
auth_algs=1
# Require clients to know the network name
ignore_broadcast_ssid=0
# Use WPA2
wpa=2
# Use a pre-shared key
wpa_key_mgmt=WPA-PSK
# The network passphrase
wpa_passphrase=PASSWORD
# Use AES, instead of TKIP
rsn_pairwise=CCMP
EOF
```

Test this access point: `hostapd /etc/hostapd/hostapd.conf`
This should print messages saying RpiEk has started. Try connecting to this using your phone. Either it goes through succesfully or it's stuck at Obtaining IP Address. Either of which, indicates a success in hostapd configuration..

Make the config and launch of hostapd permanent: Modify /etc/default/hostapd and add DAEMON_CONF="/etc/hostapd/hostapd.conf"

```bash
service hostapd restart
```
This should error out.

Unmask and enable the service: 

```bash
systemctl unmask hostapd
systemctl enable hostapd
service hostapd restart
```

Run ifconfig to check wlan1's ip address. Remember we had pointed that router is 192.168.0.1. This should be the ipv4 address showing up in `ifconfig wlan1`
If it doesn't, assign it: `ifconfig wlan1 192.168.0.1`

Route traffic using IPTables
Now, we need to enable IP forwarding in the kernel so it can forward packets for our client devices to the Internet:
```bash
sysctl -w net.ipv4.ip_forward=1
```

And change this line in `/etc/sysctl.conf` to make the changes permanent:
`net.ipv4.ip_forward = 1`

Finally, use your IPTables-magic to masquerade client connections to the Internet (which I assume is the wireless interface wlan0):
```bash
iptables -t nat -A  POSTROUTING -o wlan0 -j MASQUERADE 
```

Ensure that this change is available on each boot:
```bash
apt install iptables-persistent -y
iptables-save > /etc/iptables/rules.v4
cat > /etc/rc.local <<EOF
iptables-restore /etc/iptables/rules.v4
EOF
chmod +x /etc/rc.local
```

Check if all services are up and running:
```bash
service udhcpd restart
service hostapd restart
service udhcpd status
service hostapd status
```

You should be able to connect to RpiEk and browse the internet.
```bash
reboot
```


If client cant get ip address. Check wlan1's ipaddress. 
If client can connect by without internet access, that means the forwarding to wlan0 isn't working. Run the iptables rule again
And then restart udhcpd as many times as it takes to get it working
