# AS35661 - VIRTUASYS BGP Communities

This document describes the BGP Large Communities (RFC 8092) available for VIRTUASYS (AS35661) customers to control route announcements across our network.

## Network Locations

| Code | City | Country | POP Code |
|------|------|---------|----------|
| 000  | Paris | France | PAR01FR  |
| 001  | Lille | France | LIL01FR  |
| 010  | Frankfurt | Germany | FRA01DE  |
| 020  | Amsterdam | Netherlands | AMS01NL  |
| 999  | All Locations | - | ALL      |

## BGP Large Communities Format

All communities follow the format: `35661:action+location:parameter`

### Traffic Engineering Communities

| Community | Description |
|-----------|-------------|
| `35661:0[LOC]:[ASN]` | Route learned from ASN at location code [LOC] |
| `35661:1[LOC]:[ASN]` | Prepend 1x to ASN at location code [LOC] |
| `35661:2[LOC]:[ASN]` | Prepend 2x to ASN at location code [LOC] |
| `35661:3[LOC]:[ASN]` | Prepend 3x to ASN at location code [LOC] |
| `35661:9[LOC]:[ASN]` | Do not export to ASN at location code [LOC] |
| `35661:9[LOC]:0` | Do not export routes to location code [LOC] |

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

| ASN    | Name                      | Paris | Lille | Frankfurt | Amsterdam |
|--------|---------------------------|:-----:|:-----:|:---------:|:---------:|
| AS174  | COGENT                    |       | ✓     |           |           |
| AS1299 | ARELION                   | ✓     |       | ✓         | ✓         |
| AS3257 | GTT                       |       |       | ✓         |           |
| AS30823| AUROLOGIC                 |       |       | ✓         |           |
| AS35133| Eranium                   |       |       |           | ✓         |
| AS44530| HOPUS                     | ✓     |       |           |           |

### Internet Exchanges

| Exchange              | ASN     | City      | Country    |
|-----------------------|---------|-----------|------------|
| FranceIX Paris        | AS51706 | Paris     | France     |
| FranceIX Lille        | AS62228 | Lille     | France     |
| LILLIX                | AS47214 | Lille     | France     |
| DE-CIX Frankfurt      | AS6695  | Frankfurt | Germany    |
| DE-CIX Dusseldorf     | AS56890 | Frankfurt | Germany    |
| ERA-IX Frankfurt      | AS213687| Frankfurt | Germany    |
| ERA-IX Amsterdam      | AS206221| Amsterdam | Netherlands|

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
