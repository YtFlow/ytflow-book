# fake-ip

Assign a fake IP address for each domain name. This is useful for TUN inbounds where incoming connections carry no information about domain names.

## Access Points

- `resolver`: Resolver. Resolve domain names to fake IP addresses.

## Parameters

```json
{
  "prefix_v4": [11, 17],
  "prefix_v6": [38, 12, 32, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
  "fallback": "real-ip.resolver"
}
```

- `prefix_v4`: Specify the first two bytes of the fake IPv4 addresses. For `prefix_v4: [11, 17]`, the first IPv4 address to assign will be `11.17.0.1`.
- `prefix_v6`: Specify the first 14 bytes of the fake IPv6 addresses. For `prefix_v6: [38, 12, 32, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1]`, the first IPv6 address to assign will be `260c:2001::1:1`.
- `fallback`: Descriptor of the fallback Resolver. Used to retrieve other types of Resource Records such as SRV and TXT. *Not used as of YtFlowCore version 0.1.*

## Revision History

- 2023-04-29: Removed `map_back` related content.
