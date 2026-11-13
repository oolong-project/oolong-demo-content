# Oolong demo content

Demo catalog for [Oolong](https://github.com/oolong-project/oolong), a server for the
[Transparency Exchange API (TEA)](https://github.com/CycloneDX/transparency-exchange-api)
**1.0.0**. It carries the Kauf Bulb and Apache Log4j catalogs from
[Reliza-Demos/conferences-oolong-content-2025-11](https://github.com/Reliza-Demos/conferences-oolong-content-2025-11),
converted to the TEA 1.0.0 content layout that Oolong reads.

Serve it from Oolong with the Helm chart's Git pod (`gitPod.repoUrl`), or mount the checkout as
`CONTENT_DIR`. Build and run instructions are in the Oolong README.

## Catalog

| Product | Releases | Components |
|---|---|---|
| Kauf Bulb | BLF10-1m-1.95, BLF10-1m-1.96, BLF10-4m-1.95, BLF10-4m-1.96 | Kauf Bulb Firmware 1.95 / 1.96, Kauf Bulb Hardware BLF10 / BLF10-4m |
| Apache Log4j2 | 2.25.0, 2.25.1 | Apache Log4j API, Apache Log4j Core |
| Apache Log4j2 Core | 2.25.0, 2.25.1 | Apache Log4j Core |

Artifacts are the published CycloneDX SBOMs and hardware BOMs. Each one is referenced by URL
with its checksum, and linked (`distributionIds`) to the firmware images or JARs it describes.

## Layout

```
products/<name>/product.yaml
products/<name>/releases/<version>/release.yaml
products/<name>/releases/<version>/collections/<n>.yaml
components/<name>/component.yaml
components/<name>/releases/<version>/release.yaml           # distributions
components/<name>/releases/<version>/collections/<n>.yaml   # artifact revisions per collection version
artifacts/<uuid>/v<n>.yaml                                  # one file per artifact revision
```

Dates are UTC RFC 3339. Collections reference artifacts as `{uuid, version}`.

## Demo TEIs

The TEIs use the domain `oolong.demo.rearmhq.com`. TEI discovery reads
`https://oolong.demo.rearmhq.com/.well-known/tea`, so these resolve once the demo is served
there. A `purl` TEI carries the Package URL Base64URL-encoded without padding, as TEA 1.0.0
requires.

| TEI | Resolves to |
|---|---|
| `tei://oolong.demo.rearmhq.com/uuid/9485fbc9-aa7d-4c26-95c9-d5a8ccb1c073` | Kauf Bulb BLF10-1m-1.95 |
| `tei://oolong.demo.rearmhq.com/asin/B09D6HR559` | all four Kauf Bulb releases |
| `tei://oolong.demo.rearmhq.com/uuid/ff08fa6a-1df9-4ab7-b3ec-e02837845a06` | Apache Log4j2 2.25.0 |
| `tei://oolong.demo.rearmhq.com/purl/cGtnOm1hdmVuL29yZy5hcGFjaGUubG9nZ2luZy5sb2c0ai9sb2c0ai1jb3JlQDIuMjUuMA` (`pkg:maven/org.apache.logging.log4j/log4j-core@2.25.0`) | Apache Log4j2 Core 2.25.0 |
| `tei://oolong.demo.rearmhq.com/purl/cGtnOmdpdGh1Yi9hcGFjaGUvbG9nZ2luZy1sb2c0ajI` (`pkg:github/apache/logging-log4j2`) | both Apache Log4j2 releases |

With [ReARM CLI](https://github.com/relizaio/rearm-cli) 26.11.6 or later (TEA 1.0.0 client):

```bash
rearm tea full_tea_flow --tei tei://oolong.demo.rearmhq.com/uuid/9485fbc9-aa7d-4c26-95c9-d5a8ccb1c073
rearm tea full_tea_flow --tei tei://oolong.demo.rearmhq.com/asin/B09D6HR559
```

A TEI that resolves to several product releases asks which one to show, so run it in a
terminal. Discovery by PURL also works against the server directly, for example
`/v1.0.0/discovery?purl=pkg:maven/org.apache.logging.log4j/log4j-core@2.25.0`.

## Changes from the 2025-11 conference content

- TEA 1.0.0 layout: one file per artifact revision, `mediaType` (was `mimeType`), `createdDate`
  and `belongsTo` on collections, releases naming their product or component, a
  `distributionId` on every distribution, and `tei://` identifiers (were `urn:tei:`).
- The two Kauf Bulb hardware BOM checksums were for CRLF copies of the files. They now match
  the bytes served, and the `taleodor/kauf-bulb-boms` URLs are pinned to a commit so they can't
  change under the checksum.

## License

[MIT](LICENSE)
