# Rosetta

Event canonicalizer for the [Citadel](https://github.com/sltcnb/citadel) pipeline. Maps a `ForensicEvent` stream onto ECS v8 and OSSEM.

Parsers disagree about field names. Rosetta is the layer that makes them agree, so a timeline built from EVTX, Zeek and CloudTrail can be queried with one set of field names. Mapping is driven by config rather than code, so a new source is a field map, not a patch.

Status is partial: the normalize path and the daemon work, and the mapping coverage is still growing.

## Install

```bash
pip install git+https://github.com/sltcnb/rosetta
```

Python 3.11 or newer.

## Normalizing a file

```bash
rosetta normalize events.jsonl -o ecs.jsonl
```

Input is newline-delimited JSON of `ForensicEvent` objects. Output is newline-delimited ECS v8.

## Running as a daemon

```bash
rosetta daemon --watch ./incoming --es http://localhost:9200
```

Watches an input path and writes to Elasticsearch. Useful flags: `--watch` for the directory to follow, `--es` for the target cluster, `--map` for the field map, `--interval` for poll frequency, and `--once` to drain and exit.

## Field maps

Maps live as config and are applied per source. Examples are under [`examples/`](examples/). GeoIP enrichment is available through `geoip2` if you supply a database.

## Tests

```bash
pip install pytest pyyaml
pip install -e .
pytest -q
```

## License

[PolyForm Noncommercial 1.0.0](LICENSE). Run, modify and self-host it for any noncommercial purpose. Commercial use needs written authorization from the copyright holder; see [LICENSING.md](LICENSING.md).

This is a source-available license, not an OSI-approved open source license.

## Related

[Citadel](https://github.com/sltcnb/citadel) · [Sluice](https://github.com/sltcnb/sluice) upstream · [Sigil](https://github.com/sltcnb/sigil) and [Anvil](https://github.com/sltcnb/anvil) consume the normalized stream · [citadel-contracts](https://github.com/sltcnb/citadel-contracts)
