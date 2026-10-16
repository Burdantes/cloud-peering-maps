# Cloud direct-peering maps

Interactive maps of where Google Cloud, AWS and Microsoft Azure hand traffic to
neighboring networks, inferred from traceroutes run from inside each cloud.
Described at <https://burdantes.github.io/projects/cloud-peering-maps/>.

| Map | Provider | Campaign | Family | Regions | Neighbor ASNs | Cities | IXPs |
|---|---|---|---|---|---|---|---|
| [`gcp-2026-10-ipv4-ipv6.html`](https://burdantes.github.io/cloud-peering-maps/gcp-2026-10-ipv4-ipv6.html) | Google Cloud | `monthly/20261012-5k/gcp` | IPv4 + IPv6 | 42 | 6,679 | 2,791 | 230 |
| [`aws-2026-10-ipv4-ipv6.html`](https://burdantes.github.io/cloud-peering-maps/aws-2026-10-ipv4-ipv6.html) | AWS | `monthly/20261012-5k/aws` | IPv4 + IPv6 | 27 | 2,664 | 3,460 | 86 |
| [`azure-2026-10-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/azure-2026-10-ipv4.html) | Microsoft Azure | `monthly/20261012-5k/azure` | IPv4 | 39 | 4,665 | 3,414 | 216 |
| [`gcp-2026-09-ipv4-ipv6.html`](https://burdantes.github.io/cloud-peering-maps/gcp-2026-09-ipv4-ipv6.html) | Google Cloud | `monthly/20260915/gcp` | IPv4 + IPv6 | 43 | 6,853 | 2,696 | 232 |
| [`gcp-2026-08-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/gcp-2026-08-ipv4.html) | Google Cloud | `gcp-global42-20260803t2030z` | IPv4 | 41 | 5,208 | 1,803 | 219 |
| [`aws-2026-08-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/aws-2026-08-ipv4.html) | AWS | `aws-20260806-trace-global28` | IPv4 | 27 | 3,434 | 3,126 | 119 |
| [`azure-2026-09-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/azure-2026-09-ipv4.html) | Microsoft Azure | `monthly/20260915/azure-part1b` | IPv4 | 20 | 4,649 | 3,629 | 215 |
| [`gcp-2026-03-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/gcp-2026-03-ipv4.html) | Google Cloud | `gcp-march-20260308` | IPv4 | 42 (125 zones) | 5,879 | 3,351 | 178 |

The searchable table [`peer-networks.html`](https://burdantes.github.io/cloud-peering-maps/peer-networks.html)
lists every October 2026 neighbor network per provider with the regions that saw it,
whether it was reached across an IXP, and which other providers also see it. Its data
are `peer-networks/<provider>-2026-10.json`, built from the same per-region peer-link
files as the maps.

## Provenance

- **October 2026 campaigns.** All three clouds were measured in parallel on 12–13
  October 2026 from every eligible region, at **5,000 probes per second** per worker
  (September: 1,000). Google rate-limits ICMP replies at that rate, so Google Cloud
  counts are somewhat lower than at 1,000 pps. Azure IPv4 requested the **Internet
  (hot-potato) routing preference**; only 28 of 39 regions behaved that way. Azure IPv6
  probes failed to leave the VMs in 38 of 39 regions (a worker fault, not a network
  result), so the Azure map is IPv4 only. AWS is IPv4 + IPv6 for the first time.
  Assets: RouteViews 2026-10-12, IXP membership (EuroIX, PCH, PeeringDB) of 2026-10-13,
  IPInfo 2026-10-13.

- **Measurement.** scamper ICMP-echo traceroutes from one VM per region (March 2026: three). IPv4
  probes one address per routed /24 (12,189,812 targets). IPv6 (Google Cloud
  only) probes one address per announced prefix (254,233).
- **Routing configuration.** Google Cloud IPv4 ran on the Standard network tier;
  IPv6 ran on the Premium tier, the only tier that offers external IPv6. Azure
  public IPs used the default routing preference (Microsoft global network). AWS
  used ordinary EC2 public addresses.
- **Neighbor rule (identical for all maps).** An address listed as an IXP
  member port maps to that member's ASN; otherwise it maps to the RouteViews
  origin AS of its covering prefix, for the measurement date. A neighbor is the
  first non-provider network after a provider hop, across two consecutive
  responding hops. The Google Cloud and AWS August and Azure September campaigns were re-derived under this rule
  from their published mapping output on 2026-10-02, because their maps had been
  built before it existed. That more than doubled their neighbor counts. The March
  campaign was re-derived the same way.
- **March 2026 differs.** `gcp-2026-03-ipv4.html` comes from an earlier
  campaign with a different design: three zones in each of 42 regions (125
  VMs), 44-byte probes without payload, a gap limit of 5, and a sorted list of
  the `.1` address in each /24 (about 11.84M targets). Its network tier was not
  recorded, and its IXP membership tables are older. Compare it with the other
  maps with care.
- **Geolocation.** Neighbor interfaces are placed with IPInfo and, where a
  hostname encodes a location, CAIDA HOIHO. Router-interface geolocation can be
  off by hundreds of kilometres.
- **Background tiles.** Esri World Dark Gray Canvas (Esri, HERE, Garmin,
  © OpenStreetMap contributors). Reference layers come from PeeringDB.

Each page is self-contained: its dataset is embedded, and Leaflet loads from a
CDN with a subresource-integrity hash.
