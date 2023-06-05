# switch

Handle incoming connections using runtime-selected handlers from a pre-defined list.

> This plugin requires a database file to work. If a database file is not provided, it will fail to load.

## Access Points

- `tcp`: Stream Handler selected at runtime.
- `udp`: Datagram Session Handler selected at runtime.

## Parameters

```json
{
  "choices": [
    {
      "name": "Proxy",
      "description": "Proxy all connections unconditionally",
      "tcp_next": "proxy-forward.tcp",
      "udp_next": "proxy-forward.udp"
    },
    {
      "name": "Rule",
      "description": "Match connections against rules",
      "tcp_next": "rule-dispatcher.tcp",
      "udp_next": "rule-dispatcher.udp"
    }
  ]
}
```

- `choices`: Specify the list of choices.
  - `choices[].name`: Name of the choice.
  - `choices[].description`: Description of the choice.
  - `choices[].tcp_next`: Descriptor of the Stream Handler to use when the choice is selected.
  - `choices[].udp_next`: Descriptor of the Datagram Session Handler to use when the choice is selected.

## Details

Selection is managed by the UI application. During runtime, a user can change the selection from the UI via RPC. The selection will be stored in the database so that the plugin will automatically load the last choice on next startup.

## Revision History

- 2023-06-05: Added `switch`.
