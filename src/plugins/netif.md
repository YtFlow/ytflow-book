# netif

A dynamic network interface.

## Access Points

- `netif`: Netif.
- `resolver`: Resolver. Resolve domain names using configuration associated with this network interface.

## Parameters

```json
{
  "family_preference": "NoP",
  "type": "Auto"
}
```

- `family_preference`: Specify the preferred address family. Can be one of the following (case sensitive):
    - `NoPreference`
    - `PreferIpv4`
    - `PreferIpv6`
- `type`: Determine how to pick a network interface. Can be one of the following (case insensitive):
    - `Auto`: Select a wired, physical interface if available, otherwise a wireless interface.
    - `Manual`: Select a network interface by name.
    - `Virtual`: Pre-define all necessary properties of a network interface.
- `netif`: Specify the name of the network interface. *Only takes effect when `type` is `Manual`.*
- `netif`: Specify the parameters of the network interface. *Only takes effect when `type` is `Virtual`.*
    - `netif.name`: Name of the virtual interface.
    - `netif.dns_servers`: List of DNS servers, in the form of strings.
    - `netif.ipv4_addr` (optional): IPv4 bind address of the network interface, in string format. For example, `192.168.1.1:0`. If not present, IPv4 is disabled.
    - `netif.ipv6_addr` (optional): IPv6 bind address of the network interface, in string format. For example, `[fc00::%0]:0`. If not present, IPv6 is disabled.

## Examples

Select a network interface automatically:
```json
{
  "family_preference": "NoPreference",
  "type": "Auto"
}
```
Select the network interface called `eth0`:
```json
{
  "family_preference": "NoPreference",
  "type": "Manual",
  "netif": "eth0"
}
```
A imaginary interface:
```json
{
  "family_preference": "NoPreference",
  "type": "Virtual",
  "netif": {
    "name": "veth1",
    "ipv4_addr": "192.168.1.1:0",
    "ipv6_addr": "[fc00::1%0]:0",
    "dns_servers": ["1.1.1.1"]
  }
}
```

## Details

To keep track of usable network interfaces, the plugin keeps watching network changes regardless of `type`. On Windows, [Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/en-us/uwp/api/windows.networking.connectivity.networkinformation.networkstatuschanged) is used to listen to network changes. On Linux, [Netlink](https://man7.org/linux/man-pages/man7/netlink.7.html) is used to query network information and receive changes.
