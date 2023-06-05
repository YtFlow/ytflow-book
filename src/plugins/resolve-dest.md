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
- `tcp_next` (optional): Descriptor of the Stream Handler to handle the Stream with destination addresses resolved by the resolver. The `tcp` access point will be available only when this parameter is set.
- `udp_next` (optional): Descriptor of the Datagram Session Handler to handle the Datagram Session with destination addresses resolved by the resolver for all incoming packets. Destination addresses for outgoing packets will be mapped back to domain names based on resolution history within the session at a best-effort basis. The `udp` access point will be available only when this parameter is set.

## Revision History

- 2023-04-29: Removed `reverse`.
- 2023-06-05: Fixed `tcp_next`, `udp_next` types.
- 2023-06-05: Specify UDP mapback behavior.
