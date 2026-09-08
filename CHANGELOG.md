# Changelog

## 0.5.0 — 2026-09-07

- Define source and studyId; add creator/license metadata and update descriptive wording.
- Restrict label-collision checks to shared concept schemes; make synonym collisions advisory as documented.
- Validate indicators' measured State and inverse hierarchy assertions.
- Validate planned and reported outcome records independently of observedDirection; require ofState and check ofIndicator types.
- Add validation examples and contract tests.

- Permit evidence-supported multiple-State measurement applicability; retain required State targets and hierarchy exclusion. FetalWeight is the regression example.
- Replace SustainGraph SKOS class matches with documentary rdfs:seeAlso references.

Migration: revalidate existing vocabularies and normalized evidence records before adopting this version. Check indicator targets and inverse links for consistency, and refresh validation caches when the schema changes. No class or property IRIs are renamed. Version 0.5.0 reflects the change from exactly one measured State to one or more evidence-supported States per Indicator.

## 0.4.0

Previous extracted ontology. The file declared a modification date of 2026-06-04; the standalone repository was created in July 2026.
