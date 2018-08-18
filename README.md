there is a `FREE` option if you only have a free account and are not Premium.  
`remote 196.52.21.65 443 udp`

# Install and Configure OpenVPN with ProXPN account.

<table width="290" border="0" cellpadding="0" cellspacing="0">
  <tr>
    <td width="15">$</td>
    <td>sudo apt-get update</td>
  </tr>
  <tr>
    <td>$</td>
    <td>sudo apt-get install openvpn</td>
  </tr>
  <tr>
    <td>$</td>
    <td>sudo service openvpn stop</td>
  </tr>
  <tr>
    <td>$</td>
    <td>sudo systemctl disable openvpn.service</td>
  </tr>
</table>

## Copy config from ProXPN via FTP
  1. Grap the `proxpn.conf` from this repository.
  2. Make changes to the file to refelect the `remote` config line. **See Below**
  3. Transfer to Raspberry Pi via ftp to home folder.
  
<table width="505" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo mv proxpn.conf /etc/openvpn/proxpn.conf</td>
  </tr>
</table>

<table width="355">
    <tr>
      <td width="15">$</td>
      <td>sudo chown root:root /etc/openvpn/proxpn.conf</td>
    </tr>
</table>

<table width="235" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo nano /etc/openvpn/auth.txt</td>
  </tr>
</table>

first line **Username** for ProXPN  
second line **Password** for ProXPN  
**save file**

<table width="255" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo nano /etc/openvpn/route-up.sh</td>
  </tr>
</table>

	#!/bin/sh
	iptables -t nat -I POSTROUTING -o tun0 -j MASQUERADE
**save file**

<table width="240" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo nano /etc/openvpn/down.sh</td>
  </tr>
</table>

	#!/bin/sh
	iptables -t nat -D POSTROUTING -o tun0 -j MASQUERADE
**save file**

<table width="225" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo nano /etc/openvpn/vpn.sh</td>
  </tr>
</table>

`sudo openvpn --client --config /etc/openvpn/proxpn.conf --script-security 2`  
  
**save file**

<table width="290" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo chmod +x /etc/openvpn/down.sh</td>
  </tr>
  <tr>
    <td>$</td>
    <td>sudo chmod +x /etc/openvpn/route-up.sh</td>
  </tr>
  <tr>
    <td>$</td>
    <td>sudo chmod +x /etc/openvpn/vpn.sh</td>
  </tr>
</table>

<table width="170" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo nano /etc/rc.local</td>
  </tr>
</table>

`/etc/openvpn/vpn.sh`  
  before exit0  
**save file**

<table width="170" border="0">
  <tr>
    <td width="15">$</td>
    <td>sudo shutdown -r now</td>
  </tr>
</table>

to reboot  
  
once back up  

<table width="60" border="0">
  <tr>
    <td width="15">$</td>
    <td>ifconfig</td>
  </tr>
</table>
  
or  
  
<table width="60" border="0">
  <tr>
    <td width="15">$</td>
    <td>ip a</td>
  </tr>  
</table>
  
you should see *tun0*  
  
<table width="115" border="0">
  <tr>
    <td width="15">$</td>
    <td>./speedtest-cli</td>
  </tr>
</table>
  
from <a href="https://github.com/sivel/speedtest-cli" target="new">https://github.com/sivel/speedtest-cli</a><br />
to check ip, route, ping  
  
Once you verify you are not coming from your own ip, Bob's your uncle.  
  
- - -
 
## For the remote config line of ProXPN.conf
All exit nodes use port `443`

ex. for Miami using udp:
  `remote 196.52.22.2 443 udp`

    TCP_EXIT_NODES[NYC]=196.52.17.0
    TCP_EXIT_NODES[Miami]=192.240.98.3
    TCP_EXIT_NODES[Seattle]=50.7.74.99
    TCP_EXIT_NODES[Chicago]=50.7.1.243
    TCP_EXIT_NODES[Toronto]=162.253.128.69
    TCP_EXIT_NODES[Netherlands]=213.179.213.2
    TCP_EXIT_NODES[Stockholm]=94.185.84.34
    TCP_EXIT_NODES[London]=78.157.207.131
    TCP_EXIT_NODES[Sydney]=45.32.241.118
    TCP_EXIT_NODES[Frankfurt]=50.7.89.131
    TCP_EXIT_NODES[Frankfurt2]=37.123.112.35
    TCP_EXIT_NODES[SanJose]=179.48.248.104
   
    UDP_EXIT_NODES[FREE]=196.52.21.65
    UDP_EXIT_NODES[NYC]=196.52.16.2
    UDP_EXIT_NODES[NYC2]=196.52.17.68
    UDP_EXIT_NODES[Miami]=196.52.22.2
    UDP_EXIT_NODES[Miami2]=196.52.23.66
    UDP_EXIT_NODES[Seattle]=196.52.18.2
    UDP_EXIT_NODES[Seattle2]=196.52.19.241
    UDP_EXIT_NODES[Chicago]=50.7.1.67
    UDP_EXIT_NODES[Chicago2]=196.52.21.225
    UDP_EXIT_NODES[Toronto]=162.253.128.67
    UDP_EXIT_NODES[HongKong]=43.251.159.25
    UDP_EXIT_NODES[HongKong2]=103.194.43.131
    UDP_EXIT_NODES[HongKong3]=103.194.43.26
    UDP_EXIT_NODES[LA]=188.172.214.196
    UDP_EXIT_NODES[Netherlands]=213.179.212.2
    UDP_EXIT_NODES[Netherlands2]=213.179.208.146
    UDP_EXIT_NODES[Stockholm]=94.185.84.42
    UDP_EXIT_NODES[London]=78.157.207.139
    UDP_EXIT_NODES[Bucharest]=89.46.102.14
    UDP_EXIT_NODES[Sydney]=45.32.191.165
    UDP_EXIT_NODES[Frankfurt]=50.7.88.172
    UDP_EXIT_NODES[Frankfurt2]=37.123.112.69
    UDP_EXIT_NODES[Frankfurt3]=185.41.141.171
    UDP_EXIT_NODES[Paris]=151.236.21.12
    UDP_EXIT_NODES[Singapore]=191.101.242.121
    UDP_EXIT_NODES[SanJose]=179.48.248.105
    UDP_EXIT_NODES[Hafnarfjordur]=37.235.49.195
    UDP_EXIT_NODES[Zurich]=178.209.52.135
