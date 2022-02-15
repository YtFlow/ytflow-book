# null

Silently drop any outgoing requests.

## Access Points

- `tcp`: Stream Outbound.
- `udp`: Datagram Session Outbound.
- `resolver`: Resolver.

## Parameters

```json
null
```

## Note

The plugin returns an error for any requests, instead of being a black hole like [`/dev/null`](https://en.wikipedia.org/wiki/Null_device).
