# dns-server

Respond to DNS request messages using results returned by the specified resolver.

## Access Points

- `udp`: Datagram Session Handler. DNS messages are exchanged over the Datagram Session.

## Parameters

```json
{
  "concurrency_limit": 64,
  "ttl": 60,
  "resolver": "fake-ip.resolver"
}
```

- `concurrency_limit`: Maximum number of DNS requests the plugin can handle at a time. Exceeding this limit will cause new requests to wait.
- `ttl`: [Time-to-live](https://docs.rs/trust-dns-proto/0.20.4/trust_dns_proto/rr/resource/struct.Record.html#method.set_ttl) (TTL) of DNS responses.
- `resolver`: Descriptor of the Resolver to query DNS records.
