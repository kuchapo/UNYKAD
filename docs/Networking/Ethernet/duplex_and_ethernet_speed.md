# Duplex and Ethernet Speed

| Name               | Speed    | Supports Half Duplex? | Supports Full Duplex? |
| ------------------ | -------- | --------------------- | --------------------- |
| Ethernet           | 10 Mbps  | yes                   | yes                   |
| FastEthernet       | 100 Mbps | yes                   | yes                   |
| GigabitEthernet    | 1 Gbps   | no                    | yes                   |
| 10GigabitEthernet  | 10 Gbps  | no                    | yes                   |
| 40GigabitEthernet  | 40 Gbps  | no                    | yes                   |
| 100GigabitEthernet | 100 Gbps | no                    | yes                   |

## Notes

- **Hubs** are always *Half-Duplex* (they cannot support Full-Duplex).
- **Switches** support both *Half-Duplex and Full-Duplex*, but modern switches always use *Full-Duplex*.
