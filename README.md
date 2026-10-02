# Cloud direct-peering maps

Interactive maps of where Google Cloud, AWS and Microsoft Azure hand traffic to
neighboring networks, inferred from traceroutes run from inside each cloud.
Described at <https://burdantes.github.io/projects/cloud-peering-maps/>.

| Map | Provider | Campaign | Family | Regions | Neighbor ASNs | Cities | IXPs |
|---|---|---|---|---|---|---|---|
| [`gcp-2026-09-ipv4-ipv6.html`](https://burdantes.github.io/cloud-peering-maps/gcp-2026-09-ipv4-ipv6.html) | Google Cloud | `monthly/20260915/gcp` | IPv4 + IPv6 | 43 | 6,853 | 2,696 | 232 |
| [`gcp-2026-08-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/gcp-2026-08-ipv4.html) | Google Cloud | `gcp-global42-20260803t2030z` | IPv4 | 41 | 5,208 | 1,803 | 219 |
| [`aws-2026-08-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/aws-2026-08-ipv4.html) | AWS | `aws-20260806-trace-global28` | IPv4 | 27 | 3,434 | 3,126 | 119 |
| [`azure-2026-09-ipv4.html`](https://burdantes.github.io/cloud-peering-maps/azure-2026-09-ipv4.html) | Microsoft Azure | `monthly/20260915/azure-part1b` | IPv4 | 20 | 4,649 | 3,629 | 215 |

## Provenance

- **Measurement.** scamper ICMP-echo traceroutes from one VM per region. IPv4
  probes one address per routed /24 (12,189,812 targets). IPv6 (Google Cloud
  only) probes one address per announced prefix (254,233).
- **Routing configuration.** Google Cloud IPv4 ran on the Standard network tier;
  IPv6 ran on the Premium tier, the only tier that offers external IPv6. Azure
  public IPs used the default routing preference (Microsoft global network). AWS
  used ordinary EC2 public addresses.
- **Neighbor rule (identical for all four maps).** An address listed as an IXP
  member port maps to that member's ASN; otherwise it maps to the RouteViews
  origin AS of its covering prefix, for the measurement date. A neighbor is the
  first non-provider network after a provider hop, across two consecutive
  responding hops. The three earlier campaigns were re-derived under this rule
  from their published mapping output on 2026-10-02, because their maps had been
  built before it existed. That more than doubled their neighbor counts.
- **Geolocation.** Neighbor interfaces are placed with IPInfo and, where a
  hostname encodes a location, CAIDA HOIHO. Router-interface geolocation can be
  off by hundreds of kilometres.
- **Background tiles.** Esri World Dark Gray Canvas (Esri, HERE, Garmin,
  © OpenStreetMap contributors). Reference layers come from PeeringDB.

Each page is self-contained: its dataset is embedded, and Leaflet loads from a
CDN with a subresource-integrity hash.
