# resolve-dest

Resolve domain names in flow destinations from/to IP addresses.

## Access Points

- `tcp`: Stream Handler. Available only when `tcp_next` is set. Try to replace the destination domain name with an IP address it resolves to (or vice versa).
- `udp`: Datagram Session Handler. Available only when `udp_next` is set. For Datagram Session Handlers and every datagram flowing through, try to replace the destination domain name with an IP address it resolves to (or vice versa).

## Parameters

```json
{
  "resolver": "fake-ip.resolver",
  "reverse": true,
  "tcp_next": "main-forward.tcp",
  "udp_next": "main-forward.tcp",
}
```

- `resolver`: Descriptor of the Resolver.
- `reverse`: Specify forward resolution (from domain name to IP address) or reverse resolution (from IP address to domain name).
- `tcp_next` (optional): Descriptor of the Stream Outbound to establish new outbound Streams. The `tcp` access point will be available only when this parameter is set.
- `udp_next` (optional): Descriptor of the Datagram Session Outbound to establish new outbound Datagram Sessions. The `udp` access point will be available only when this parameter is set.
