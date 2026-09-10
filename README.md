# Universal Evidence Ontology

An OWL/RDFS schema with SHACL validation rules for describing studies and organizing the controlled vocabularies used by [Universal Evidence](https://universalevidence.com).

**Version:** 0.5.0. The reviewed indicator-link inconsistencies have been resolved, and this version is deployed in Universal Evidence. See CHANGELOG.md.

## Eight classes

| Class | Meaning |
| --- | --- |
| State | A state of wellbeing, health, development, or a social, ecological, economic or institutional system. A study can address it as a condition or measure it as an outcome. |
| Intervention | A deliberate action intended to change a condition. |
| Subject | The kind of entity that bears a state, such as a population or system. |
| Indicator | An operationalized metric applicable to one or more evidence-supported States. |
| OutcomeMeasurement | A study-specific outcome record identifying the State measured, optionally its indicator, direction, intent and reporting status. A planned outcome need not have a measured direction. |
| Region | A geographic entity, including countries, subdivisions and multi-country groupings. |
| Evidence | A record of a study, trial or evaluation, with provenance and links to vocabulary concepts. A registration does not by itself establish an intervention's effectiveness. |
| CrosswalkEntry | A mapping record used to connect source terminology to normalized vocabulary concepts. |

The ontology records associations and stated objectives; an evidence connection does not assert causation or a beneficial effect.

## Files and reuse

- `ue.ttl`: schema, metadata and embedded SHACL rules.
- `examples/`: small valid and invalid data examples.
- `tests/`: executable contract tests.
- `requirements-validation.txt`: tested validation dependencies.

The document identifier is <https://universalevidence.com/ontology/>. Terms retain stable IRIs beneath that namespace. The file can also be loaded directly, without a network connection. External alignment identifiers are references, not bundled external ontologies; no OWL imports are required.

Specific concepts live in the [Conditions and Outcomes thesaurus](https://github.com/SteveHedden/universalevidence-conditions-outcomes-thesaurus) and [Interventions thesaurus](https://github.com/SteveHedden/universalevidence-interventions-thesaurus). The Conditions and Outcomes repository also carries Subject concepts; Indicators use a distinct class within that vocabulary. Both companion repositories are public.

## Validate data

```sh
python -m venv .venv
. .venv/bin/activate
pip install -r requirements-validation.txt
pyshacl -s ue.ttl -m --allow-warnings examples/valid.ttl
pyshacl -s ue.ttl -m --allow-warnings examples/invalid.ttl
pytest -q
```

The valid example passes; the invalid example exits with nonconformance because an outcome does not identify a State. The `-m` option checks the shapes themselves. `--allow-warnings` follows UE's policy that warnings are advisory; violations fail. Use a SHACL-SPARQL capable validator. Load referenced vocabulary types together with your data: validation cannot recognize an external target as a State without its type declaration.

An Indicator must have at least one effective measured State, using `measures`, inverse `measuredBy`, or both consistently. Multiple measured States are permitted when the metric supports each interpretation: fetal weight can contribute to assessing restricted and excessive fetal growth. This does not assert either diagnosis or their simultaneous presence. An outcome records its particular State through `ofState`; applications must not infer every possible diagnosis from an indicator link. Indicators do not participate in broader/narrower hierarchies. Core State and Intervention preferred labels must be unique within a shared scheme; reuse in a separate scheme is allowed. Current label cardinality is a monolingual UE policy.

Evidence uses `condition` for a State and `outcome` for an OutcomeMeasurement, which links to its State through `ofState`. Optional `ofIndicator` identifies an Indicator. `source` identifies the source registry/dataset and `studyId` identifies its record. Source identifiers may be literals or IRIs; study identifiers are strings. These fields are a normalized record contract: source-native mirrors may use different fields and must be normalized before claiming conformance.

## Validation boundaries

The embedded shapes check structural contracts, not the scientific correctness of a mapping, whether an intervention works, or the quality of a study. CrosswalkEntry and Region do not yet have comprehensive standalone conformance shapes. Cycle detection and related-link reciprocity are checked by UE's construction tooling, not these embedded shapes. Vocabulary SKOS mappings and the soft BFO annotations should not be read as fully axiomatized equivalences. SustainGraph class references use rdfs:seeAlso rather than SKOS concept matching; the generic State class is not a close match for ClimateHazard.

Before changing an existing deployment, compare validation findings on an unchanged data snapshot under both ontology versions. Resolve newly exposed data problems before activating stricter constraints. Revalidate cached data when the ontology changes, and keep schema and vocabulary updates consistent. Source-native registry records must be normalized before validation against the Evidence contract.

## License and attribution

Ontology content is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); see LICENSE. Suggested attribution: “Universal Evidence Ontology, Steve Hedden, version 0.5.0, CC BY 4.0,” with a link to this repository. Indicate changes when distributing an adaptation. The license on UE application code is separate.

The ontology was extracted from UE's application repository. Changes are tracked in Git; release notes appear in CHANGELOG.md.

## Validation examples

The included examples and tests cover valid and invalid outcome records, multiple-State indicator applicability, and other structural constraints. They demonstrate the normalized ontology contract; they do not certify external datasets or replace review of scientific mappings.
