# AS35661 - VIRTUASYS BGP Communities

This document describes the BGP Large Communities (RFC 8092) available for VIRTUASYS (AS35661) customers to control route announcements across our network.

**Note:** This is a work in progress. Our goal is to replace all legacy BGP communities in the `35661:` namespace with BGP Large Communities.
During the transition period, both legacy and Large Community formats may coexist.

## Network Locations

| Location Code | City          | Country     | POP Code |
|---------------|---------------|-------------|----------|
| 000           | Paris         | France      | PAR01FR  |
| 001           | Lille         | France      | LIL01FR  |
| 010           | Frankfurt     | Germany     | FRA01DE  |
| 020           | Amsterdam     | Netherlands | AMS01NL  |
| 999           | All Locations | -           | ALL      |

## BGP Large Communities Format

All communities follow the format: `35661:ACTION+LOCATION:TARGET`

- **ACTION**: Single digit defining the operation (first digit of the second field)
- **LOCATION**: 3-digit location code (see table above)
- **TARGET**: Target ASN, or a reserved group selector

### Actions

| Action | Community            | Description                                      |
|--------|----------------------|--------------------------------------------------|
| `0`    | `35661:0[LOC]:[TGT]` | Route learned from TARGET at LOCATION             |
| `1`    | `35661:1[LOC]:[TGT]` | Prepend 1x to TARGET at LOCATION                  |
| `2`    | `35661:2[LOC]:[TGT]` | Prepend 2x to TARGET at LOCATION                  |
| `3`    | `35661:3[LOC]:[TGT]` | Prepend 3x to TARGET at LOCATION                  |
| `8`    | `35661:8[LOC]:[TGT]` | Export only to TARGET at LOCATION                 |
| `9`    | `35661:9[LOC]:[TGT]` | Do not export to TARGET at LOCATION               |

Actions 4-7 are reserved for future use.

### Target Group Selectors

| Target | Meaning                          |
|--------|----------------------------------|
| `0`    | All peers at location            |
| `1`    | All **transit** peers at location |
| `2`    | All **IX** peers at location      |
| ASN    | Specific peer (by ASN)           |

**Note:** Group selectors use reserved ASN values (AS0 per RFC 7607, AS1, AS2) that are not real peering targets.

### Precedence Rules

When multiple communities apply to the same route, they are evaluated from highest to lowest priority:

1. `35661:8[LOC]:ASN` -- Explicit allow (always wins)
2. `35661:9[LOC]:ASN` -- Explicit deny per-ASN
3. `35661:9[LOC]:1` / `35661:9[LOC]:2` -- Group deny (transit / IX)
4. `35661:9[LOC]:0` -- Location deny (all peers)
5. `35661:9999:0` -- Global deny

## Network Peers

### Transit Providers

| ASN     | Name      | Paris | Lille | Frankfurt | Amsterdam |
|---------|-----------|:-----:|:-----:|:---------:|:---------:|
| AS174   | COGENT    |   ⌛   |   ✓   |           |           |
| AS1299  | ARELION   |   ✓   |       |     ✓     |     ⌛     |
| AS3257  | GTT       |       |       |     ✓     |           |
| AS30823 | AUROLOGIC |       |       |     ✓     |           |
| AS35133 | Eranium   |       |       |           |     ✓     |
| AS44530 | HOPUS     |   ✓   |       |           |           |

### Internet Exchanges

| Exchange          | ASN      | City      | Country     |
|-------------------|----------|-----------|-------------|
| FranceIX Paris    | AS51706  | Paris     | France      |
| FranceIX Lille    | AS62228  | Lille     | France      |
| LILLIX            | AS47214  | Lille     | France      |
| ERA-IX Frankfurt  | AS213687 | Frankfurt | Germany     |
| ERA-IX Amsterdam  | AS206221 | Amsterdam | Netherlands |

## IX Peers

| ASN      | Peer Name   | FranceIX Paris | LILLIX | ERA-IX Frankfurt | ERA-IX Amsterdam |
|----------|-------------|:--------------:|:------:|:----------------:|:----------------:|
| AS714    | Apple       |       ✓        |        |                  |                  |
| AS6939   | HE.NET      |       ✓        |        |        ✓         |        ✓         |
| AS8075   | Microsoft   |       ✓        |        |                  |                  |
| AS8966   | ETISALAT    |       ✓        |        |                  |                  |
| AS9009   | M247        |                |        |                  |        ✓         |
| AS13335  | CLOUDFLARE  |       ✓        |        |        ✓         |        ✓         |
| AS15169  | GOOGLE      |       ✓        |        |        ✓         |        ✓         |
| AS16276  | OVH         |       ✓        |   ✓    |                  |                  |
| AS20940  | AKAMAI      |       ✓        |        |                  |                  |
| AS25369  | ZARE        |       ✓        |        |                  |                  |
| AS32934  | META        |       ✓        |        |                  |        ✓         |
| AS34019  | HIVANE      |       ✓        |   ✓    |                  |                  |
| AS62044  | ZSCALER     |       ✓        |        |                  |                  |
| AS197922 | TECHCREA    |                |   ✓    |                  |                  |
| AS206002 | SCALAIR     |                |   ✓    |                  |                  |

## Examples

```
# Do not export to CLOUDFLARE (AS13335) at Paris (location code 000)
35661:9000:13335

# Prepend 2x to ARELION (AS1299) in Frankfurt (location code 010)
35661:2010:1299

# Prepend 3x to all peers at all locations
35661:3999:0

# Do not export routes to Amsterdam (location code 020)
35661:9020:0

# Export routes only to ARELION (AS1299) in Paris (location code 000)
35661:9999:0         # Step 1: Do not export anywhere
35661:8000:1299      # Step 2: Allow export only to ARELION in Paris

# Export routes only to GOOGLE (AS15169) in Frankfurt (location code 010)
35661:9999:0         # Step 1: Do not export anywhere
35661:8010:15169     # Step 2: Allow export only to GOOGLE in Frankfurt

# Block export to Lille (location code 001) but allow to COGENT (AS174) there
35661:9001:0         # Step 1: Do not export to Lille
35661:8001:174       # Step 2: Allow export only to COGENT in Lille

# Anycast: block all IX peers at Paris, keep transits, allow only CLOUDFLARE on IX
35661:9000:2         # Step 1: Block all IX peers at Paris
35661:8000:13335     # Step 2: Exception: allow CLOUDFLARE at Paris

# Block all IX peers globally but keep transits everywhere
35661:9999:2         # Block all IX peers at all locations
```
