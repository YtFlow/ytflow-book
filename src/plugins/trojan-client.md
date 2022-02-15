# trojan-client

Trojan client.

## Access Points

- `tcp`: Stream Outbound.

*The UDP protocol is not implemented as of YtFlowCore 0.1.*

## Parameters

```json
{
  "password": {
    "__byte_repr": "utf8",
    "data": "my_trojan_password"
  },
  "tls_next": "trojan-client-tls.tcp"
}
```

- `password`: Password.
- `tls_next`: Descriptor of the Stream Outbound to send **plaintext** Trojan requests.

## Note

TLS is not included. You will likely need to connect `trojan-client` to a `tls-client`.
