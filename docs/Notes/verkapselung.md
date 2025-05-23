# Verkapselung
## Transport Layer

Beispiel anhand von Transmission Control Protocol (TCP):

| Source Port | Destination Port | Flags  | Seq    | Ack    | Payload |
| ----------- | ---------------- | ------ | ------ | ------ | ------- |
| HEADER      | HEADER           | HEADER | HEADER | HEADER | DATA    |

Das ganze bezeichnet man als `SEGMENT`.

## Network Layer

Beispiel anhand von IPv4:

| Source IP Adress | Destination IP Adress | TTL    | Other  | Segment |
| ---------------- | --------------------- | ------ | ------ | ------- |
| HEADER           | HEADER                | HEADER | HEADER | DATA    |

Das ganze bezeichnet man als `PACKET`.

## Data Link Layer

Beispiel anhand von Ethernet:

| Destination MAC Adress | Source MAC Adress | Layer 3 Protocol | Packet |
| ---------------------- | ----------------- | ---------------- | ------ |
| HEADER                 | HEADER            | HEADER           | DATA   |

Das ganze bezeichnet man als `FRAME`.