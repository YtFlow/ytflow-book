# list-dispatcher

Match the connection against a list of matchers defined in a resource, and use the handler of the action or fallback handler if there is no match.

## Access Points

- `tcp`: Stream Handler defined in `action.tcp` and selected by the rules, or `fallback.tcp` if none matches.
- `udp`: Datagram Session Handler defined in `action` and selected by the rules, or `fallback.udp` if none matches. Dispatching applies to Datagram Sessions only. Datagrams flowing through are not inspected.
- `resolver`: Resolver defined in `actions` and the requesting domain name is selected by the rules, or `fallback.resolver` if none matches.

## Parameters

```json
{
  "source": "ad-domain-list",
  "action": {
    "tcp": "reject.tcp",
    "udp": "reject.udp",
    "resolver": "null.resolver"
  },
  "fallback": {
    "tcp": "proxy-forward.tcp",
    "udp": "proxy-forward.udp",
    "resolver": "fake-ip.resolver"
  }
}
```

- `source`: Specify an existing ruleset or inline rules. Use a string for the name of the ruleset, or an object for inline rules. See [Rule Source](#rule-source) section for more details.
  - `source.format`: Specify the format of the inline rules. Can be one of the following:
      - [`surge-domain-set`](#surge-domain-set)
  - `source.text`: An array of the lines of the inline rules, excluding line endings.
- `action`: Specify the Stream Handler, Datagram Session Handler and Resolver to use when the list matches the request.
  - `actions.tcp` (optional): Descriptor of the Stream Handler to handle Streams when there is a match. If omitted, Streams will be discarded when matched.
  - `actions.udp` (optional): Descriptor of the Datagram Session Handler to handle Datagram Sessions when there is a match. If omitted, Datagram Sessions will be discarded when matched.
  - `actions.resolver` (optional): Descriptor of the Resolver to use when the requested domain name is matched by the list. If omitted, no response will be generated when matched.
- `fallback`: The action to use when no rule matches. See `action` for the format.

## Rule Source

The `source` parameter specifies the ruleset or inline rules for matching. The format can be one of the following:

### `surge-domain-set`

`surge-domain-set` is a line-based text format. Each line is a either a full domain name, or a domain suffix preceded by a dot (`.`).

```
# This is a comment.
.lan
localhost.ptlogin2.qq.com
```

This ruleset will match `my-pc.lan`, `localhost.ptlogin2.qq.com`, but not `my-pc.lan.com` or `my-pc.localhost.ptlogin2.qq.com`.

## Examples

Use `system-resolver` for specified domains, and `fake-ip` for the rest.

```json
{
  "action": {
    "resolver": "system-resolver.resolver"
  },
  "fallback": {
    "resolver": "fake-ip.resolver"
  },
  "source": {
    "format": "surge-domain-set",
    "text": [
      ".msftconnecttest.com",
      ".msftncsi.com",
      ".lan",
      "localhost.ptlogin2.qq.com"
    ]
  }
}
```

Reject connections to domain names listed in `loyalsoldier-surge-reject`:

```json
{
  "action": {
    "tcp": "reject.tcp",
    "udp": "reject.udp"
  },
  "fallback": {
    "tcp": "proxy-forward.tcp",
    "udp": "resolve-proxy.udp"
  },
  "source": "loyalsoldier-surge-reject"
}
```

## Revision History

- 2024-04-05: Added `list-dispatcher`.
