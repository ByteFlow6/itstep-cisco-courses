# Port Security

## What is it and why is it needed?

Port Security is a feature that restricts which devices can connect to a switch port by limiting the number of MAC addresses allowed on that port.

It is used to prevent **MAC Flooding** attacks — when an attacker sends thousands of frames with fake MAC addresses to overflow the switch's CAM table.

## How it works

1. You configure the **maximum number** of MAC addresses allowed on a port.
2. If the number of addresses exceeds the limit, the port takes action based on the **violation mode**.
3. In all modes, the attacker's traffic is blocked.

## Violation Modes

| Mode | Action | Log? | Port State |
|------|--------|------|------------|
| `shutdown` | Port goes to err-disabled | Yes | Down |
| `restrict` | Drops packets, sends syslog | Yes | Up |
| `protect` | Drops packets, no syslog | No | Up |

### My take on each mode:
- **`shutdown`** — the most radical. Good for military networks, where any hint of intrusion requires immediate action.
- **`restrict`** — balanced. Good for regular office networks — the port keeps working but blocks the attacker and logs the event.
- **`protect`** — for ports where uptime is critical (IP phones, cameras). The port doesn't go down, doesn't log, just silently blocks.

## Configuration Example

```cisco
interface fa0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
