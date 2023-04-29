# netif

A dynamic network interface.

## Access Points

- `netif`: Netif.
- `resolver`: Resolver. Resolve domain names using configuration associated with this network interface.

## Parameters

```json
{
  "family_preference": "Ipv4Only",
  "type": "Auto"
}
```

- `family_preference`: Specify the preferred address family. Can be one of the following (case sensitive):
    - `Both`
    - `Ipv4Only`
    - `Ipv6Only`
- `type`: Determine how to pick a network interface. Can be one of the following (case insensitive):
    - `Auto`: Select a wired, physical interface if available, otherwise a wireless interface.
    - `Manual`: Select a network interface by name.
- `netif`: Specify the name of the network interface. *Only takes effect when `type` is `Manual`.*

## Examples

Select a network interface automatically:
```json
{
  "family_preference": "Both",
  "type": "Auto"
}
```
Select the network interface called `eth0`:
```json
{
  "family_preference": "Ipv4Only",
  "type": "Manual",
  "netif": "eth0"
}
```

## Details

To keep track of usable network interfaces, the plugin keeps watching network changes regardless of `type`. On Windows, [Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/en-us/uwp/api/windows.networking.connectivity.networkinformation.networkstatuschanged) is used to listen to network changes. On Linux, [Netlink](https://man7.org/linux/man-pages/man7/netlink.7.html) is used to query network information and receive changes.

When `family_preference` is set to `Auto` and there is at least one IPv6 address and one IPv4 address assigned to the network interface, the plugin utilizes [RFC 8305](https://datatracker.ietf.org/doc/html/rfc8305): Happy Eyeballs Version 2 strategy to establish a connection to both IPv4 and IPv6 networks in a concurrent manner.

## Revision History

- 2023-04-29: Removed `netif` access point.
- 2023-04-29: Refactored parameters; removed `Virtual`.
- 2023-04-29: Happy Eyeballs Version 2.
