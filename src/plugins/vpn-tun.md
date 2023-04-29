# vpn-tun

An instance to be instantiated by a VPN system service, such as UWP VPN Plugin on Windows.

## Access Points

- `tun`: TUN Interface.

## Parameters

```json
{
  "ipv4": "192.168.3.1",
  "ipv4_route": [
    "11.17.0.0/16",
    "11.16.0.0/16"
  ],
  "ipv6": null,
  "ipv6_route": [],
  "dns": ["11.16.1.1"],
  "web_proxy": null
}
```

- `ipv4` (optional): IPv4 address of the TUN interface.
- `ipv4_route`: IPv4 routes [CIDR] format.
- `ipv6` (optional): IPv6 address of the TUN interface.
- `ipv6_route`: IPv6 routes [CIDR] format.
- `dns`: Assigned DNS servers.
- `web_proxy` (optional): Assigned proxy address.

## Details

For a `vpn-tun` to work, the YtFlowCore instance should be launched by a system VPN service, such as UWP VPN Plugin on Windows. There must be at most one `vpn-tun` instance in a YtFlowCore instance.

[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing

## Revision History

- 2023-04-29: Use human representation for IP CIDR.
