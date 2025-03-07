# AS35661 - VIRTUASYS BGP Communities

This document describes the BGP Large Communities (RFC 8092) available for VIRTUASYS (AS35661) customers to control route announcements across our network.

## Network Locations

| Code | Location       | POP Code |
|------|----------------|----------|
| 0    | Paris, France  | PAR01FR  |
| 1    | Lille, France  | LIL01FR  |
| 10   | Frankfurt, Germany | FRA01DE  |
| 20   | Amsterdam, Netherlands | AMS01NL  |
| 999  | All Locations  | ALL      |

## BGP Large Communities Format

All communities follow the format: `35661:action:parameter`

### Traffic Engineering Communities

| Community | Description |
|-----------|-------------|
| `35661:0[POP]:[ASN]` | Route learned from ASN at location [POP] |
| `35661:1[POP]:[ASN]` | Prepend 1x to ASN at location [POP] |
| `35661:2[POP]:[ASN]` | Prepend 2x to ASN at location [POP] |
| `35661:3[POP]:[ASN]` | Prepend 3x to ASN at location [POP] |
| `35661:9[POP]:[ASN]` | Do not export to ASN at location [POP] |
| `35661:9[POP]:0` | Do not export routes to location [POP] |

### Internet Exchange Communities

| Community | Description |
|-----------|-------------|
| `35661:8001:0` | Do not export to all FranceIX Paris peers |
| `35661:8002:0` | Do not export to all FranceIX Lille peers |
| `35661:8003:0` | Do not export to all LILLIX peers |
| `35661:8004:0` | Do not export to all DE-CIX Frankfurt peers |
| `35661:8005:0` | Do not export to all DE-CIX Dusseldorf peers |
| `35661:8006:0` | Do not export to all ERA-IX Frankfurt peers |
| `35661:8007:0` | Do not export to all ERA-IX Amsterdam peers |

## Network Peers

### Direct Peers

| ASN    | Name                      | PAR01FR | LIL01FR | FRA01DE | AMS01NL |
|--------|---------------------------|:-------:|:-------:|:-------:|:-------:|
| AS174  | COGENT                    |         | ✓       |         |         |
| AS1299 | ARELION                   | ✓       |         | ✓       | ✓       |
| AS3257 | GTT                       |         |         | ✓       |         |
| AS30823| AUROLOGIC                 |         |         | ✓       |         |
| AS35133| Eranium                   |         |         |         | ✓       |
| AS44530| HOPUS                     | ✓       |         |         |         |

### Internet Exchanges

| Exchange              | ASN     | Location    |
|-----------------------|---------|-------------|
| FranceIX Paris        | AS51706 | PAR01FR     |
| FranceIX Lille        | AS62228 | LIL01FR     |
| LILLIX                | AS47214 | LIL01FR     |
| DE-CIX Frankfurt      | AS6695  | FRA01DE     |
| DE-CIX Dusseldorf     | AS56890 | FRA01DE     |
| ERA-IX Frankfurt      | AS213687| FRA01DE     |
| ERA-IX Amsterdam      | AS206221| AMS01NL     |

## IX Peers

| ASN     | Peer Name       | FranceIX Paris | LILLIX | DE-CIX Frankfurt | DE-CIX Dusseldorf | ERA-IX Frankfurt | ERA-IX Amsterdam |
|---------|-----------------|:--------------:|:------:|:----------------:|:-----------------:|:----------------:|:----------------:|
| AS714   | Apple           | ✓              |        | ✓                |                   |                  | ✓                |
| AS8075  | Microsoft       | ✓              |        | ✓                |                   |                  | ✓                |
| AS8966  | ETISALAT        | ✓              |        | ✓                |                   |                  |                  |
| AS9009  | M247            |                |        | ✓                |                   |                  |                  |
| AS13335 | CLOUDFLARE      | ✓              |        | ✓                | ✓                 | ✓                | ✓                |
| AS15169 | GOOGLE          | ✓              |        | ✓                |                   | ✓                | ✓                |
| AS16276 | OVH             | ✓              | ✓      | ✓                |                   |                  | ✓                |
| AS20940 | AKAMAI          | ✓              |        | ✓                |                   |                  |                  |
| AS25369 | ZARE            | ✓              |        |                  |                   |                  |                  |
| AS32934 | META            | ✓              |        | ✓                | ✓                 |                  | ✓                |
| AS34019 | HIVANE          | ✓              | ✓      | ✓                |                   |                  |                  |
| AS45102 | ALIBABA         |                |        | ✓                |                   |                  |                  |
| AS62044 | ZSCALER         | ✓              |        | ✓                |                   |                  |                  |
| AS197922| TECHCREA        |                | ✓      |                  |                   |                  |                  |
| AS206002| SCALAIR         |                | ✓      |                  |                   |                  |                  |

## Examples

```
# Do not export to CLOUDFLARE (AS13335) at Paris
35661:9000:13335

# Prepend 2x to ARELION (AS1299) in Frankfurt
35661:2010:1299

# Do not export routes to any peer at DE-CIX Frankfurt (except Route Server)
35661:8004:0

# Prepend 3x to all peers at all locations
35661:3999:0

# Do not export routes to Amsterdam location
35661:9020:0
