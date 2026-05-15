# Structure Fingerprint Evidence Extensions v0.1

A minimal evidence-object layer for comparing Structure Fingerprints, expressing lineage relations, and preparing evidence for allocation review.

This repository defines a step-by-step bridge from structural fingerprinting to downstream review workflows.

It does **not** determine authorship, origin, plagiarism, infringement, royalty shares, payment execution, or legal liability.

---

## Overview

Structure Fingerprint records describe structured evidence about a text, work, idea, specification, protocol, or other creative object.

This repository adds the next evidence layers:

```text
Structure-Fingerprint
  ↓
Comparison-Result
  ↓
Lineage-Relation
  ↓
Allocation-Readiness
  ↓
Allocation / Royalty Layer

The central principle is simple:

Similarity is not authorship.
Lineage is not judgment.
Allocation-readiness is not allocation.

Each layer keeps uncertainty explicit and prevents premature enforcement.

Core Principle

This repository preserves a strict separation between:

proof-oriented evidence
inference-oriented evidence
lineage interpretation
allocation review readiness
final allocation or legal decision

In other words:

This project defines evidence objects, not verdicts.

Specifications
1. Comparison Result v0.1

comparison-result-v0.1 defines a machine-readable object for comparing two Structure Fingerprint records.

It separates:

proof comparison
inference comparison
uncertainty
review requirements
allowed and prohibited downstream uses

It does not determine authorship, origin, plagiarism, infringement, royalty allocation, or legal claims.

Status

Implemented as:

YAML specification
JSON Schema
sample JSON
pass vector
fail vectors
GitHub Actions validation
Main files
spec/comparison-result-v0.1.yaml
schemas/comparison-result-v0.1.schema.json
examples/comparison-result.sample.json
tests/vectors/pass/comparison-result.valid.json
tests/vectors/fail/comparison-result.missing-confidence.json
tests/vectors/fail/comparison-result.auto-royalty-assignment-missing-prohibition.json
2. Lineage Relation v0.1

lineage-relation-v0.1 defines a machine-readable object for expressing an evidence-backed lineage relationship between fingerprinted works or records.

It may describe relations such as:

derived_from
adapted_from
influenced_by
structurally_related
near_duplicate
shared_origin_possible
common_ancestor_possible
independent_convergence
conflicting_lineage_claim
insufficient_evidence
unrelated

It does not prove authorship, origin, plagiarism, infringement, allocation entitlement, or royalty shares.

Lineage Relation is an intermediate evidence object.
It may support review, audit, dispute handling, lineage graph construction, or allocation-readiness assessment, but it must not be used as an automatic enforcement mechanism.

Status

Implemented as:

YAML specification
JSON Schema
sample JSON
pass vector
fail vectors
GitHub Actions validation
Main files
spec/lineage-relation-v0.1.yaml
schemas/lineage-relation-v0.1.schema.json
examples/lineage-relation.sample.json
tests/vectors/pass/lineage-relation.valid.json
tests/vectors/fail/lineage-relation.missing-confidence.json
tests/vectors/fail/lineage-relation.directionality-violation.json
tests/vectors/fail/lineage-relation.missing-prohibited-use.json
3. Allocation Readiness v0.1

allocation-readiness-v0.1 defines a machine-readable object for deciding whether an evidence package is ready to enter allocation review.

It does not assign royalties or execute payments.

It only answers:

Is this evidence package ready for allocation review?

Allocation Readiness is the bridge between evidence objects and downstream allocation or royalty systems.

Status

Draft stage.

Implemented:

YAML specification draft

Planned:

JSON Schema
sample JSON
pass vector
fail vectors
GitHub Actions validation
Main file
spec/allocation-readiness-v0.1.yaml
Repository Structure
.
├── spec/
│   ├── comparison-result-v0.1.yaml
│   ├── lineage-relation-v0.1.yaml
│   └── allocation-readiness-v0.1.yaml
│
├── schemas/
│   ├── comparison-result-v0.1.schema.json
│   └── lineage-relation-v0.1.schema.json
│
├── examples/
│   ├── comparison-result.sample.json
│   └── lineage-relation.sample.json
│
├── tests/
│   └── vectors/
│       ├── pass/
│       │   ├── comparison-result.valid.json
│       │   └── lineage-relation.valid.json
│       │
│       └── fail/
│           ├── comparison-result.missing-confidence.json
│           ├── comparison-result.auto-royalty-assignment-missing-prohibition.json
│           ├── lineage-relation.missing-confidence.json
│           ├── lineage-relation.directionality-violation.json
│           └── lineage-relation.missing-prohibited-use.json
│
├── .github/
│   └── workflows/
│       └── validate-specs.yml
│
├── LICENSE
└── README.md
Start Here

If you are new to this repository, read the files in this order:

Step 1: Comparison
spec/comparison-result-v0.1.yaml
schemas/comparison-result-v0.1.schema.json
examples/comparison-result.sample.json
tests/vectors/pass/comparison-result.valid.json
tests/vectors/fail/comparison-result.missing-confidence.json
tests/vectors/fail/comparison-result.auto-royalty-assignment-missing-prohibition.json
Step 2: Lineage
spec/lineage-relation-v0.1.yaml
schemas/lineage-relation-v0.1.schema.json
examples/lineage-relation.sample.json
tests/vectors/pass/lineage-relation.valid.json
tests/vectors/fail/lineage-relation.missing-confidence.json
tests/vectors/fail/lineage-relation.directionality-violation.json
tests/vectors/fail/lineage-relation.missing-prohibited-use.json
Step 3: Allocation Readiness
spec/allocation-readiness-v0.1.yaml

This order follows the evidence flow:

compare → relate → prepare for review
Schema Usage

The current JSON Schema implementation covers:

schemas/comparison-result-v0.1.schema.json
schemas/lineage-relation-v0.1.schema.json

Both schemas use JSON Schema Draft 2020-12.

Comparison Result Schema

comparison-result-v0.1.schema.json validates the shape of a Comparison Result object.

It checks that:

proof comparison and inference comparison are separated
uncertainty confidence is present
ambiguity level is explicit
automatic authorship claims are prohibited
automatic origin claims are prohibited
automatic royalty assignment is prohibited
automatic legal claims are prohibited
unresolved references cannot have low ambiguity
sensitive comparison classes require human review
Lineage Relation Schema

lineage-relation-v0.1.schema.json validates the shape of a Lineage Relation object.

It checks that:

required namespaces are present
source and target subjects are explicit
comparison result references can be attached as evidence
relation type is expressed using controlled enum values
directional lineage claims must have directionality
confidence is present
ambiguity level is explicit
strong lineage claims require human review
high or unresolved ambiguity prevents overly strong relation strength
automatic authorship claims are prohibited
automatic origin claims are prohibited
automatic plagiarism claims are prohibited
automatic royalty assignment is prohibited
automatic legal claims are prohibited
Local Validation

Install dependencies:

python -m pip install --upgrade pip
pip install jsonschema

Validate the implemented schemas, samples, pass vectors, and fail vectors:

python - <<'PY'
import json
import sys
from pathlib import Path

from jsonschema import Draft202012Validator, FormatChecker

VALIDATION_TARGETS = [
    {
        "name": "Comparison Result v0.1",
        "schema": Path("schemas/comparison-result-v0.1.schema.json"),
        "sample": Path("examples/comparison-result.sample.json"),
        "pass_glob": "comparison-result*.json",
        "fail_glob": "comparison-result*.json",
        "required": [
            Path("tests/vectors/pass/comparison-result.valid.json"),
            Path("tests/vectors/fail/comparison-result.missing-confidence.json"),
            Path("tests/vectors/fail/comparison-result.auto-royalty-assignment-missing-prohibition.json"),
        ],
    },
    {
        "name": "Lineage Relation v0.1",
        "schema": Path("schemas/lineage-relation-v0.1.schema.json"),
        "sample": Path("examples/lineage-relation.sample.json"),
        "pass_glob": "lineage-relation*.json",
        "fail_glob": "lineage-relation*.json",
        "required": [
            Path("tests/vectors/pass/lineage-relation.valid.json"),
            Path("tests/vectors/fail/lineage-relation.missing-confidence.json"),
            Path("tests/vectors/fail/lineage-relation.directionality-violation.json"),
            Path("tests/vectors/fail/lineage-relation.missing-prohibited-use.json"),
        ],
    },
]

PASS_DIR = Path("tests/vectors/pass")
FAIL_DIR = Path("tests/vectors/fail")


def load_json(path: Path):
    try:
        with path.open("r", encoding="utf-8") as f:
            return json.load(f)
    except json.JSONDecodeError as e:
        print(f"ERROR: Invalid JSON in {path}: {e}")
        raise


def validate_should_pass(validator, path: Path) -> bool:
    instance = load_json(path)
    errors = sorted(
        validator.iter_errors(instance),
        key=lambda e: list(e.path)
    )

    if errors:
        print(f"ERROR: Expected PASS but validation failed: {path}")
        for error in errors:
            location = ".".join(str(p) for p in error.path) or "<root>"
            print(f"  - {location}: {error.message}")
        return False

    print(f"OK: {path} is valid.")
    return True


def validate_should_fail(validator, path: Path) -> bool:
    instance = load_json(path)
    errors = sorted(
        validator.iter_errors(instance),
        key=lambda e: list(e.path)
    )

    if not errors:
        print(f"ERROR: Expected FAIL but validation passed: {path}")
        return False

    print(f"OK: {path} failed as expected.")
    for error in errors[:5]:
        location = ".".join(str(p) for p in error.path) or "<root>"
        print(f"  - {location}: {error.message}")

    return True


all_ok = True

for target in VALIDATION_TARGETS:
    print("")
    print(f"=== Validating {target['name']} ===")

    required_paths = [
        target["schema"],
        target["sample"],
        *target["required"],
    ]

    for path in required_paths:
        if not path.exists():
            print(f"ERROR: Required path not found: {path}")
            all_ok = False

    if not all(path.exists() for path in required_paths):
        continue

    schema = load_json(target["schema"])
    validator = Draft202012Validator(
        schema,
        format_checker=FormatChecker()
    )

    print("")
    print("=== Validating sample ===")
    all_ok = validate_should_pass(validator, target["sample"]) and all_ok

    print("")
    print("=== Validating pass vectors ===")
    pass_vectors = sorted(PASS_DIR.glob(target["pass_glob"]))
    if not pass_vectors:
        print(f"ERROR: No pass vectors found for {target['name']}")
        all_ok = False

    for path in pass_vectors:
        all_ok = validate_should_pass(validator, path) and all_ok

    print("")
    print("=== Validating fail vectors ===")
    fail_vectors = sorted(FAIL_DIR.glob(target["fail_glob"]))
    if not fail_vectors:
        print(f"ERROR: No fail vectors found for {target['name']}")
        all_ok = False

    for path in fail_vectors:
        all_ok = validate_should_fail(validator, path) and all_ok

if all_ok:
    print("")
    print("Validation succeeded.")
    sys.exit(0)

print("")
print("Validation failed.")
sys.exit(1)
PY
GitHub Actions Validation

This repository includes a validation workflow:

.github/workflows/validate-specs.yml

The workflow validates:

schemas/comparison-result-v0.1.schema.json
examples/comparison-result.sample.json
tests/vectors/pass/comparison-result.valid.json
tests/vectors/fail/comparison-result.missing-confidence.json
tests/vectors/fail/comparison-result.auto-royalty-assignment-missing-prohibition.json

schemas/lineage-relation-v0.1.schema.json
examples/lineage-relation.sample.json
tests/vectors/pass/lineage-relation.valid.json
tests/vectors/fail/lineage-relation.missing-confidence.json
tests/vectors/fail/lineage-relation.directionality-violation.json
tests/vectors/fail/lineage-relation.missing-prohibited-use.json

Expected behavior:

sample files must pass
pass vectors must pass
fail vectors must fail

This ensures that each schema validates both valid examples and intentionally invalid boundary cases.

What JSON Schema Validates

The current schemas validate structural requirements such as:

required namespaces
required fields
allowed enum values
score ranges from 0 to 1
URI formats
date-time formats
prohibited downstream use requirements
human review requirements
directionality requirements
ambiguity and confidence requirements
What Requires Semantic Validation

Some rules are intentionally left for semantic validation rather than JSON Schema.

Examples:

weighting profile values should sum to 1.0
overall similarity should be consistent with component scores
comparison class should match configured score thresholds
matched fields and conflicting fields should refer to real field paths
confidence should reflect upstream uncertainty
lineage direction should be checked against actual timestamps and provenance records
relation strength should be consistent with the evidence bundle
alternative lineage hypotheses should be compared against external evidence
allocation readiness should be determined only after lineage review

These checks should be implemented later in a semantic validator.

Recommended future file:

tools/validate_semantics.py
Policy Boundary

This repository intentionally prohibits automatic use of these objects for:

authorship claims
origin claims
plagiarism claims
infringement claims
royalty assignment
payment execution
legal claims
irreversible enforcement

The objects may support:

review
audit
dispute handling
lineage graph construction
provenance review
allocation-readiness assessment

They must not replace human, institutional, legal, or governance review.

Design Philosophy

The project follows five design principles:

Compare before claiming lineage.
Preserve uncertainty across layers.
Separate evidence from judgment.
Keep allocation outside evidence objects.
Prevent automatic enforcement from weak evidence.

This creates a safer path from structural similarity to downstream review.

Current Status
Comparison Result v0.1:
  YAML spec: implemented
  JSON Schema: implemented
  sample JSON: implemented
  pass vector: implemented
  fail vectors: implemented
  GitHub Actions validation: implemented

Lineage Relation v0.1:
  YAML spec: implemented
  JSON Schema: implemented
  sample JSON: implemented
  pass vector: implemented
  fail vectors: implemented
  GitHub Actions validation: implemented

Allocation Readiness v0.1:
  YAML spec: draft
  JSON Schema: planned
  sample JSON: planned
  validation vectors: planned
  GitHub Actions validation: planned
Roadmap

Recommended next steps:

Add schemas/allocation-readiness-v0.1.schema.json
Add examples/allocation-readiness.sample.json
Add allocation-readiness pass/fail vectors
Extend validate-specs.yml for allocation-readiness
Add semantic validation scripts
Add relationship documentation between:
Structure Fingerprint
Comparison Result
Lineage Relation
Allocation Readiness
Royalty OS / Allocation Layer
Add a one-page overview diagram
Add release tag v0.1.0 after all three evidence layers are implemented
Non-Goals

This repository does not:

determine absolute authorship
determine absolute origin
prove infringement
prove plagiarism
assign royalty shares
execute payments
replace legal review
replace institutional governance

It defines evidence objects for structured review.

License

This repository is released under the license specified in LICENSE.

Current repository license:

MIT License
Citation

If you reference this repository, cite it as an early draft specification for evidence-object layers built on Structure Fingerprint records.

Suggested citation label:

Structure Fingerprint Evidence Extensions v0.1

Suggested citation text:

Structure Fingerprint Evidence Extensions v0.1:
Evidence-object extensions for comparison, lineage, and allocation-readiness layers in structured review workflows.
