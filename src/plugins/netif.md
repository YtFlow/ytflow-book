# netif

A dynamic network interface.

## Access Points

- `tcp`: Stream Outbound. Initiate a TCP connection bound to the selected network interface. Domain names will be resolved using `outbound_resolver` or the `resolver` of this plugin.
- `udp`: Datagram Outbound. Send a UDP packet bound to the selected network interface. Domain names will be resolved using `outbound_resolver` or the `resolver` of this plugin. 
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
- `outbound_resolver` (optional): 

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

To keep track of usable network interfaces, the plugin keeps watching network changes regardless of `type`.

When `family_preference` is set to `Auto` and there is at least one IPv6 address and one IPv4 address assigned to the network interface, the plugin utilizes [RFC 8305](https://datatracker.ietf.org/doc/html/rfc8305): Happy Eyeballs Version 2 strategy to establish a TCP connection to both IPv4 and IPv6 networks in a concurrent manner. For Datagram Sessions, there is no guarantee which address family will be used to send packets.

### Platform-specific Implementation Details

| Platform | Socket bind | Network change monitoring | Host name resolution |
| --- | --- | --- | --- |
| macOS/iOS | `IP_BOUND_IF` / `IPV6_BOUND_IF` | [`nw_path_monitor_t`](https://developer.apple.com/documentation/network/nw_path_monitor_t?language=objc) | [dnssd](https://developer.apple.com/documentation/dnssd?language=objc) |
| Windows | socket bind to interface address | [`Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged`](https://learn.microsoft.com/en-us/uwp/api/windows.networking.connectivity.networkinformation.networkstatuschanged) | [`host_resolver`](./host-resolver.md) via DNS addresses of the interface |
| Linux | `SO_BINDTODEVICE` | [`netlink`](https://man7.org/linux/man-pages/man7/netlink.7.html) | [systemd-resolved D-Bus API](https://www.freedesktop.org/software/systemd/man/latest/org.freedesktop.resolve1.html), <br> fallback to [`host-resolver`](./host-resolver.md) via DNS addresses of the interface retrieved from [NetworkManager D-Bus API](https://networkmanager.dev/docs/api/latest/gdbus-org.freedesktop.NetworkManager.DnsManager.html) |

## Revision History

- 2023-04-29: Removed `netif` access point.
- 2023-04-29: Refactored parameters; removed `Virtual`.
- 2023-04-29: Happy Eyeballs Version 2.
- 2024-04-05: Added `outbound_resolver`.
