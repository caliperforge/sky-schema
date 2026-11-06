# sky-schema

The one record shape shared by CaliperForge's sky products: [neo-watch](https://github.com/caliperforge/neo-watch) (near objects: asteroid candidates on the MPC confirmation page) and [rubin-alert-monitor](https://github.com/caliperforge/rubin-alert-monitor) (far objects: transients from the Rubin brokers).

`schema/object.json` is JSON Schema 2020-12. The Python jobs validate every record they write against it; the pages validate a fixture against it in their tests. If a field name changes, a test fails before a page breaks.

- `fixtures/near.json`: a confirmation-page candidate as the poller writes it, before the nightly job fills `urgency` and `motion`.
- `fixtures/far.json`: a contested transient with three broker labels and no name yet.

Rules:
- Positions are degrees, ICRS. Times are UTC, ISO 8601.
- A near record never carries `labels`, `truth` or `verdict`; a far record never carries `score`, `arc_days`, `outcome` or `stations`. The schema enforces both.
- `schema_version` is bumped only when a consumer must change; adding an optional field is not a bump.

Check locally:

```
pip install check-jsonschema
check-jsonschema --schemafile schema/object.json fixtures/*.json
```
