# vmess-client

VMess client.

## Access Points

- `tcp`: Stream Outbound.

*The UDP protocol is not implemented as of YtFlowCore 0.3.*

## Parameters

```json
{
  "user_id": "b831381d-6324-4d53-ad4f-8cda48b30811",
  "security": "auto",
  "alter_id": "0",
  "tcp_next": "proxy-redir.tcp"
}
```

- `user_id`: Specify the user ID to generate encryption keys.
- `security` (optional): The encryption method, or cipher for stream payload. Defaults to `auto`. Supported values:
    - `none`
    - `auto` (`aes-128-gcm` on x86_64 or aarch64 devices, `chacha20-poly1305` otherwise)
    - `aes-128-cfb`
    - `aes-128-gcm`
    - `chacha20-poly1305`
- `alter_id` (optional): `0` to enable VMess AEAD header format (default), any value greater than `0` to disable VMess AEAD header format. This value works solely as a switch for VMess AEAD header format. No alter IDs are generated.
- `tcp_next`: Descriptor of the Stream Outbound to establish new outbound Streams.

## Details

The following request options are enabled in YtFlowCore:

- `RequestOptionChunkStream` (0x01)
- `RequestOptionChunkMasking` (0x04)

The following request options are not supported:

- `RequestOptionConnectionReuse` (0x02)
- `RequestOptionGlobalPadding` (0x08)
- `RequestOptionAuthenticatedLength` (0x10)

## Revision History

- 2023-05-11: Added `vmess-client`.
