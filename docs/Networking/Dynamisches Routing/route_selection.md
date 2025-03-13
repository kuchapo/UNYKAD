# Route Selection (Route Preference)

Der Router berechnet normalerweise die beste Route anhand von:

1. Prefix Length (smallest subnet preferred), e.g. a /32 is preferred over a /30
2. Highest Administrative Distance, e.g. OSPF routes are preferred over RIP routes
3. Lowest Metric, e.g. OSPF routes with metric of 100 are preferred over the routes that have 200 metric cost

## Admin Distances in Cisco

| **Route Source**                         | **Admin Distance** |
|------------------------------------------|--------------------|
| Direkt verbundene Netzwerke              | 0                  |
| Static Route                             | 1                  |
| EIGRP                                    | 90                 |
| OSPF                                     | 110                |
| IS-IS                                    | 115                |
| RIP                                      | 120                |
| iBGP                                     | 200                |
| **Unreachable (nicht vertrauenswürdig)** | **255**            |
