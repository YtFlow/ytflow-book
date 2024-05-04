# rule-dispatcher

Match the connection against rules defined in a resource, and use the handler of a corresponding action or fallback handler if there is no match.

## Access Points

- `tcp`: Stream Handler defined in `actions` and selected by the rules, or `fallback.tcp` if none matches.
- `udp`: Datagram Session Handler defined in `actions` and selected by the rules, or `fallback.udp` if none matches. Dispatching applies to Datagram Sessions only. Datagrams flowing through are not inspected.
- `resolver`: Resolver defined in `actions` and the requesting domain name is selected by the rules, or `fallback.resolver` if none matches.

## Parameters

```json
{
  "resolver": "doh-resolver.resolver",
  "source": "enhanced-proxy-list",
  "geoip": "dreamacro-geoip",
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

- `resolver` (optional): Specify the resolver to resolve domain names to IP addresses as additional inputs for rule matching. If omitted, destination addresses with domain names will not be matched against IP address related rules. A secure DNS resolver is recommended. See [`host-resolver`](./host-resolver.md) for a DNS-over-HTTPS resolver.
- `geoip` (optional): Specify the resource name of a GeoIP database to use for matching. If omitted, the rules with type `geoip` in `quanx-filter` rulesets will be skipped during matching. The GeoIP database is a MaxMind DB File containing country data. Do not specify this parameter if the ruleset has type `geoip-country` which is redundant and will not take effect.
- `source`: Specify an existing ruleset or inline rules. Use a string for the name of the ruleset, or an object for inline rules. See [Rule Source](#rule-source) section for more details.
  - `source.format`: Specify the format of the inline rules. Can be one of the following:
      - [`quanx-filter`](#quanx-filter)
  - `source.text`: An array of the lines of the inline rules, excluding line endings.
- `actions`: Specify the groups of Stream Handlers, Datagram Session Handlers and Resolvers.
  - `actions[].tcp` (optional): Descriptor of the Stream Handler to handle Streams when the action is selected. If omitted, Streams will be discarded when the action is selected.
  - `actions[].udp` (optional): Descriptor of the Datagram Session Handler to handle Datagram Sessions when the action is selected. If omitted, Datagram Sessions will be discarded when the action is selected.
  - `actions[].resolver` (optional): Descriptor of the Resolver to use when the action is selected. If omitted, no response will be generated for resolution requests.
- `rules`: Specify the rule-action mapping. The meaning of the keys depends on the type of the ruleset. For `geoip-country`, the keys are [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country codes (case-insensitive). For `quanx-filter`, the keys are the targets of the ruleset.
- `fallback`: The action to use when no rule matches. See `actions` for the format.

## Rule Source

The `source` parameter specifies the ruleset or inline rules for matching. The format can be one of the following:

### `geoip-country`

A MaxMind DB File containing country data. The rules are matched against the country code of the destination IP address. It cannot be used as inline rules.

### `quanx-filter`

`quanx-filter` is a line-based text format. Each line consists of a few comma-separated components specifying the matching criteria and a target. The target is used as the key in the `rules` parameter.

```ini
; This is a comment.
# This is also a comment.

; Take action `direct` when the destination domain name is exactly www.example.com
host, www.example.com, direct
domain, www.example.com, direct # equivalent to the above line

; Take action `proxy` when the destination domain name ends with ip.sb
host-suffix, ip.sb, proxy
domain-suffix, ip.sb, proxy # equivalent to the above line

; Take action `direct` when the destination domain name contains google-analytics
host-keyword, google-analytics, reject
domain-keyword, google-analytics, reject # equivalent to the above line

; Take action `direct` when the destination IP address is 114.114.114.114.
; Skipped if the destination address is a domain name.
ip-cidr, 114.114.114.114/32, direct, no-resolve

# Take action `direct` when the destination IP address is in the range.
; If the destination address is a domain name, the IP address resolved by the resolver parameter will be matched.
ip-cidr, 1.1.1.1/16, direct

; Take action `proxy` when the destination IP address is in the range 2001:4860:4860::8800/120.
; Skipped if the destination address is a domain name.
ip6-cidr, 2001:4860:4860::8800/120, proxy, no-resolve
ip-cidr6, 2001:4860:4860::8800/120, proxy, no-resolve ; equivalent to the above line

; take action `direct` when the destination IP address is located in China.
; If the destination address is a domain name, the resolved IP address will be matched.
geoip, cn, direct

; take action `proxy` for all unmatched connections.
; This line is **optional**.
final, proxy
```

The first component specifies the type of the rule:
- `host`/`domain`: Match the exact domain name of the destination against the second component. Take the action specified in the third component if matched.
- `host-suffix`/`domain-suffix`: Match the suffix of the domain name of the destination against the second component. Take the action specified in the third component if matched.
- `host-keyword`/`domain-keyword`: Match the keyword in the domain name of the destination against the second component. Take the action specified in the third component if matched.
- `ip-cidr`: Match the destination IP address against the CIDR block specified in the second component. Take the action specified in the third component if matched. If the fourth component `no-resolve` exists, the rule is skipped when the destination address is a domain name.
- `ip6-cidr`/`ip-cidr6`: Match the destination IPv6 address against the CIDR block specified in the second component. Take the action specified in the third component if matched. If the fourth component `no-resolve` exists, the rule is skipped when the destination address is a domain name.
- `geoip`: Match the destination IP address against the country code specified in the second component. Take the action specified in the third component if matched. If the fourth component `no-resolve` exists, the rule is skipped when the destination address is a domain name. The GeoIP database is specified in the `geoip` parameter. The rule is skipped if the `geoip` parameter is not set.
- `final`: Take the action specified in the second component for all unmatched connections. As opposed to other implementations, the `final` rule is optional. When omitted, the `fallback` action is used for all unmatched connections.

During matching, the rules are evaluated in the order they appear in the ruleset. The first rule that matches the connection is selected. A host name resolution is not performed until it reaches an `ip-cidr`, `ip6-cidr`, or `geoip` rule without `no-resolve`.

## Examples

Dispatch connections targeting IP addresses located in China to `direct-forward` [`forward`](./forward.md) plugin using a managed GeoIP database:

```json
{
  "actions": {
    "direct": {
      "tcp": "direct-forward.tcp",
      "udp": "resolve-local.udp"
    }
  },
  "fallback": {
    "tcp": "proxy-forward.tcp",
    "udp": "resolve-proxy.udp"
  },
  "resolver": "doh-resolver.resolver",
  "rules": {
    "cn": "direct"
  },
  "source": "dreamacro-geoip"
}
```

Dispatch connections based on custom inline rules:

```json
{
  "actions": {
    "direct": {
      "tcp": "direct-forward.tcp",
      "udp": "resolve-local.udp"
    },
    "proxy": {
      "tcp": "proxy-forward.tcp",
      "udp": "resolve-proxy.udp"
    },
    "reject": {
      "tcp": "reject.tcp",
      "udp": "reject.udp"
    }
  },
  "fallback": {
    "tcp": "geoip-dispatcher.tcp",
    "udp": "geoip-dispatcher.udp"
  },
  "rules": {
    "direct": "direct",
    "proxy": "proxy",
    "reject": "reject"
  },
  "source": {
    "format": "quanx-filter",
    "text": [
      "domain, www.example.com, direct",
      "domain-suffix, ip.sb, proxy",
      "domain-keyword, google-analytics, reject",
      "ip-cidr, 114.114.114.114/32, direct, no-resolve",
      "ip6-cidr, 2001:4860:4860::8800/120, proxy, no-resolve"
    ]
  }
}
```

## Revision History

- 2023-06-05: Added `rule-dispatcher`.
- 2024-04-05: Added `quanx-filter`.
