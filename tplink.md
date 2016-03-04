Downloaded [OpenWrt firmware image](https://downloads.openwrt.org/chaos_calmer/15.05/ar71xx/generic/openwrt-15.05-ar71xx-generic-tl-wr1043nd-v2-squashfs-factory.bin).

In browser, went to router web admin at [http://192.168.0.1/](http://192.168.0.1/).
Default username/password is: `admin`/`admin`.
Went to `System Tools` -> `Firmware Upgrade` to upgrade the firmware to OpenWrt.

Renamed `openwrt-15.05-ar71xx-generic-tl-wr1043nd-v2-squashfs-factory.bin` to `openwrt.bin`.
Selected the file, upgraded the firmware. Waited for it to reboot.

After reboot:

```
telnet 192.168.1.1
```

Set password with `passwd` command.

```
ssh -l root 192.168.1.1
```

In `/etc/config/system` configure the hostname.

In `/etc/config/network` configured the network:

```
config interface 'lan'                         
        option ifname 'eth1'                   
        option force_link '1'         
        option type 'bridge'          
        option proto 'static'         
        option ipaddr '10.20.144.1'   
        option netmask '255.255.252.0'
        option ip6assign '60'         
```

Disabled IPv6 DHCP:

```
uci set dhcp.lan.dhcpv6=disabled
uci set dhcp.lan.ra=disabled
```

Set DHCP IP pool:

```
uci set dhcp.lan.start=100
uci set dhcp.lan.limit=859
```

```
uci commit dhcp
```
```
reboot
```
```
ssh -l root 10.20.144.1
```

Check everything:

```
ifconfig
ps w
```
