# host-resolver

Resolve real IP addresses by querying DNS servers.

## Access Points

- `resolver`: Resolver. Resolve domain names to real IP addresses, and map recent results back to domain names on a best-effort basis.

## Parameters

```json
{
  "udp": ["redir-8888.udp", "redir-8844.udp"],
  "tcp": ["redir-8888.tcp", "redir-8844.tcp"],
}
```

- `udp`: Descriptor of the Datagram Session Outbounds to send DNS requests.
- `tcp`: Descriptor of the Stream Outbounds to send DNS requests. *Not used as of YtFlowCore 0.1.*

## Details

The `host-resolver` plugin is powered by [`trust_dns_resolver`](https://docs.rs/trust-dns-resolver/latest/trust_dns_resolver/).
