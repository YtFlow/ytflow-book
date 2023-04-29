# socket

Represents a system socket connection.

## Access Points

- : Stream Outbound and Datagram Session Outbound. Initiate a TCP connection or UDP socket. Use the plugin name as the descriptor.

## Parameters

```json
{
  "resolver": "phy.resolver",
  "bind_addr_v4": "0.0.0.0:0",
  "bind_addr_v6": null
}
```

- `resolver`: Descriptor of the Resolver to resolve domain names to IP addresses.
- `bind_addr_v4` (optional): IPv4 socket address to bind to. If omitted, a default socket address will be used. If set to `null`, IPv4 will be disabled.
- `bind_addr_v6` (optional): IPv6 socket address to bind to. If omitted, a default socket address will be used. If set to `null`, IPv6 will be disabled.

## Details

When both `bind_addr_v4` and `bind_addr_v6` are non-`null` (i.e. either specified a value or omitted), the plugin utilizes [RFC 8305](https://datatracker.ietf.org/doc/html/rfc8305): Happy Eyeballs Version 2 strategy to establish a connection to both IPv4 and IPv6 networks in a concurrent manner.

## Revision History

- 2023-04-29: Removed `netif`; added `bind_addr`.
- 2023-04-29: Happy Eyeballs Version 2.
