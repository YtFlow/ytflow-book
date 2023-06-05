# rule-dispatcher

Match the connection against rules defined in a resource, and use the handler of a corresponding action or fallback handler if there is no match.

## Access Points

- `tcp`: Stream Handler defined in `actions` and selected by the rules, or `fallback` if none matches.
- `udp`: Datagram Session Handler defined in `actions` and selected by the rules, or `fallback` if none matches. Dispatching applies to Datagram Sessions only. Datagrams flowing through are not inspected.
- `resolver`: Resolver defined in `actions` and selected by the rules, or `fallback` if none matches.

## Parameters

```json
{
  "resolver": "doh-resolver.resolver",
  "source": "dreamacro-geoip",
  "actions": {
    "direct": {
      "tcp": "direct-forward.tcp",
      "udp": "direct-forward.udp",
      "resolver": "phy.resolver"
    },
    "reject": {
      "tcp": "reject.tcp",
      "udp": "reject.udp",
      "resolver": "null.resovler"
    }
  },
  "rules": {
    "cn": "direct"
  },
  "fallback": {
    "tcp": "proxy-forward.tcp",
    "udp": "proxy-forward.udp",
    "resolver": "fake-ip.resolver"
  }
}
```

- `resolver` (optional): Specify the resolver to resolve domain names to IP addresses as additional inputs for rule matching. It omitted, destination addresses with domain names will not be matched against IP address related rules. A secure DNS resolver is recommended. See [`host-resolver`](./host-resolver.md) for a DNS-over-HTTPS resolver.
- `source`: Specify the name of the ruleset. (TODO: resource?)
- `actions`: Specify the groups of Stream Handlers, Datagram Session Handlers and Resolvers.
  - `actions[].tcp` (optional): Descriptor of the Stream Handler to handle Streams when the action is selected. If omitted, Streams will be discarded when the action is selected.
  - `actions[].udp` (optional): Descriptor of the Datagram Session Handler to handle Datagram Sessions when the action is selected. If omitted, Datagram Sessions will be discarded when the action is selected.
  - `actions[].resolver` (optional): Descriptor of the Resolver to use when the action is selected. If omitted, no response will be generated for resolution requests.
- `rules`: Specify the rule-action mapping. The meaning of the keys depends on the type of the ruleset. For `geoip-country`, the keys are [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country codes (case-insensitive).
- `fallback`: The action to use when no rule matches. See `actions` for the format.

## Revision History

- 2023-06-05: Added `rule-dispatcher`.
