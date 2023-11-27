# Networking

## IPv6 address delegation

To delegate a prefix assigned from your ISP to a local subnet (do directly connected to your
router), here are athe steps to make it with work with both `dnsmasq` and `NetworkManager`.

The setup used is the following:

```

+-------------+               +-----------------+                    +----------------+
| 10.0.0.2/24 |<-- net-vm --->| 10.0.0.1/24     |                    |                |
| VM          |               | Qemu host       |                    | ISP's box      |
|             |               | 192.168.0.13/24 |<-- physical net -->| 192.168.0.1/24 |
+-------------+               +-----------------+                    +----------------+
```

1. Make sure you _do_ have a valid IPv6 stack on host. Typically `ipv6.method` can be `auto`.

2. Create the bridge network on host with `nmcli`
   ```bash
   nmcli connection add type bridge \
        ifname vms0 con-name vms0 autoconnect yes save yes stp no \
        ipv6.method shared \
        ipv4.method static ipv4.address 10.0.0.1/24
   nmcli connection up vms0
   ```

3. Configure IPv6 address delegation for `dnsmasq`
   ```dnsmasq
   interface=vms0
   enable-ra
   dhcp-range=::,constructor:vms0,1h
   dhcp-option=option6:dns-server,::
   dhcp-option=option6:ntp-server,::
   ```
