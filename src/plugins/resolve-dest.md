# resolve-dest

Resolve domain names in flow destinations to IP addresses.

## Access Points

- `tcp`: Stream Handler. Available only when `tcp_next` is set. Try to replace the destination domain name with an IP address it resolves to.
- `udp`: Datagram Session Handler. Available only when `udp_next` is set. For Datagram Session Handlers and every datagram flowing through, try to replace the destination domain name with an IP address it resolves to.

## Parameters

```json
{
  "resolver": "fake-ip.resolver",
  "tcp_next": "main-forward.tcp",
  "udp_next": "main-forward.tcp",
}
```

- `resolver`: Descriptor of the Resolver.
- `tcp_next` (optional): Descriptor of the Stream Outbound to establish new outbound Streams. The `tcp` access point will be available only when this parameter is set.
- `udp_next` (optional): Descriptor of the Datagram Session Outbound to establish new outbound Datagram Sessions. The `udp` access point will be available only when this parameter is set.

## Revision History

- 2023-04-29: Removed `reverse`.
