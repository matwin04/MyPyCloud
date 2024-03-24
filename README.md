# PiCloud
 Python WiFi Bridge

## Using this wiki
**table**
- [Hello](#basic-bridge)
# Basic Bridge
## 1 Update/Upgrade
`sudo apt update`
Update App Package
`sudo apt upgrade`
Upgrade App Package
## Install Packages
`sudo apt-get install dnsmasq iptables`

## 3 Setup WLAN 0

`sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

## 4 WPA_SUPPLICANT Configuration

`network={
ssid="networkname"
psk="pswd"
}`

## 5 Configure ETH0
`sudo nano /etc/dhcpcd.conf`

### interface eth0
|interface eth0||
|-|-|
|static ip|192.168.10.100|
|router|192.168.10.100|

**This table is for refrence only!!!**
### ifconfig
`interface eth0
static ip_address=192.168.220.1/24
static routers=192.168.220.0`

## 8 Restart Service
`sudo service dhcpd restart`
## 9 Make Backup (Optional but recomended)
`sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig`

## 10 Create new conf file
`sudo nano /etc/dnsmasq.conf`
## 11 DNS / DHCP Traffic
### Refrence Table
|item|value|description|
|-|-|-|
|interface|eth0|use interface eth0
listen-address|192.168.xx.xx|adress to listen on|
bind-dynamic|-|bind to the interface|
server|8.8.8.8|use google|
domain-needed|-|dont forward short names|
bogus-priv|-|drop the non-routed address spaces|
dhcp-range|192.168.xxx.xx 192.168.xxx.xx 12H|IP Range lease tim in hours|


### Add to File

## 12 Firewall
### 12-1 open file
`sudo nano /etc/sysctl.conf`
### 12-2
`#net.ipv4.ip_forward=1`
**Replace With**
`net.ipv4.ip_forward=1`

## 13 Enable IPV4
`sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"`
##  14 Add Rules to IPTable
### 14.1
`sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE`
### 14.2
`sudo iptables -A FORWARD -i wlan0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT`
### 14.3
`sudo iptables -A FORWARD -i eth0 -o wlan0 -j ACCEPT`
## 15 Save to every boot
`sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"`

## 16 load to every boot
### 16.1 Open File
`sudo nano /etc/rc.local`
### 16.2 Find where to add
find `exit 0`
### 16.3
**add above**`exit 0`
`iptables-restore < /etc/iptables.ipv4.nat`
## 17 DNS Masq
`sudo service dnsmasq start`
## 18 Reboot

# Cloud NAS
[Link To Source](https://opensource.com/article/18/9/host-cloud-nas-raspberry-pi?ref=itsfoss.com)
## 1 Install Nextcloud
### 1.1 Install necesarry packages
`sudo apt install unzip wget php apache2 mysql-server php-zip php-mysql php-dom php-mbstring php-gd php-curl`
### 1.2 Install Nextcloud
#### Make Nextcloud Directory
`sudo mkdir -p /nas/data/nextcloud`
#### 2
## 