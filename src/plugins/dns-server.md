# dns-server

Respond to DNS request messages using results returned by the specified resolver. Also provides domain name lookup (`map_back`) for resolved IP addresses.

## Access Points

- `udp`: Datagram Session Handler. DNS messages are exchanged over the Datagram Session.
- `tcp_map_back.[tcp_map_back]`: Stream Handlers for each descriptor defined in `tcp_map_back`. Streams with IP addresses as their destinations are redirected to the corresponding domain name based on DNS resolution history.
- `udp_map_back.[udp_map_back]`: Datagram Session Handlers for each descriptor defined in `udp_map_back`. Datagrams with IP addresses as their destinations are redirected to the corresponding domain name based on DNS resolution history.

## Parameters

```json
{
  "concurrency_limit": 64,
  "ttl": 60,
  "resolver": "fake-ip.resolver",
  "tcp_map_back": ["main-forward.tcp"],
  "udp_map_back": ["main-forward.udp"]
}
```

- `concurrency_limit`: Maximum number of DNS requests the plugin can handle at a time. Exceeding this limit will cause new requests to wait.
- `ttl`: [Time-to-live](https://docs.rs/trust-dns-proto/0.20.4/trust_dns_proto/rr/resource/struct.Record.html#method.set_ttl) (TTL) of DNS responses.
- `resolver`: Descriptor of the Resolver to query DNS records.
- `tcp_map_back`: List of descriptors of Stream Handlers. Each descriptor specified here will become an access point of the plugin (e.g. `tcp_map_back.main-forward.tcp`), using which a stream with an IP address as its destination can be redirected to the corresponding domain name based on DNS resolution history, and passed down to the specified access point.
- `udp_map_back`: List of descriptors of Datagram Session Handlers. Each descriptor specified here will become an access point of the plugin (e.g. `udp_map_back.main-forward.udp`), using which a datagram with an IP address as its destination can be redirected to the corresponding domain name based on DNS resolution history, and passed down to the specified access point.

## Revision History

- 2023-04-29: Added `map_back` related content.
