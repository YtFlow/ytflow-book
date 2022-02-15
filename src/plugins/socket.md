# socket

Represents a system socket connection.

## Access Points

- : Stream Outbound and Datagram Session Outbound. Initiate a TCP connection or UDP socket. Use the plugin name as the descriptor.

## Parameters

```json
{
  "netif": "phy.netif",
  "resolver": "phy.resolver"
}
```

- `netif`: Descriptor of the network interface to use.
- `resolver`: Descriptor of the Resolver to resolve domain names to IP addresses.
