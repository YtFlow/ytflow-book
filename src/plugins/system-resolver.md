# system-resolver

Resolve real IP addresses by calling system functions. This is the recommended resolver for simple proxy scenarios for both client and server.

## Access Points

- `resolver`: Resolver. Resolve domain names to real IP addresses, and map recent results back to domain names on a best-effort basis.

## Parameters

```json
null
```

## Details

The implementation uses [`tokio::net::lookup_host`](https://docs.rs/tokio/latest/tokio/net/fn.lookup_host.html), which invokes [getaddrinfo](https://man7.org/linux/man-pages/man3/getaddrinfo.3.html) under the hood.
