# 15 — Demo Playbook

This walkthrough is designed for a reviewer with five minutes after installation. For a review without installation, use the [two-minute reading path](00-Start-Here.md#two-minute-review-without-installation).

## Before the demonstration

Complete the [README quickstart](../README.md#quickstart) with Python 3.11–3.13 and run from the repository root. The core walkthrough only needs `python -m pip install -e .`; the optional engineering checks below need the development dependencies. Installation and package builds are outside the five-minute walkthrough.

On Windows PowerShell without an activated environment, use `.\.venv\Scripts\soc-replay.exe` in place of `soc-replay`, and `.\.venv\Scripts\python.exe` in place of `python`.

## Minute 0–1: establish the thesis

Open the checked-in [`reference/network-scan/report.md`](../reference/network-scan/report.md). Show the decision summary: seven synthetic events, one detection, five supporting event IDs, and one simulated recommendation. Explain that the review question is whether the result can be traced and reproduced. The physical lab provides research context; this runnable example uses synthetic fixtures.

## Minute 1–2: inspect the wiring

```bash
soc-replay doctor
soc-replay graph --format mermaid
soc-replay explain scenarios/network-scan
```

Show the five stages, authorization boundary, and exact expected detection. The output also exposes contract versions, registered adapters, the plan fingerprint, and candidate selectors for deeper technical inspection.

## Minute 2–3: execute and verify

```bash
soc-replay verify-bundle reference/network-scan --source scenarios/network-scan
soc-replay run scenarios/network-scan --output build/network-scan
soc-replay verify-bundle build/network-scan --source scenarios/network-scan
```

Highlight that the committed reference bundle reproduces byte for byte before generating a fresh copy. Then show the PASS verdict, exact evidence-event IDs, rule execution trace, candidate intersection, distinct-port threshold, plan fingerprint, ledger root, bundle ID, and simulation-only response.

## Minute 3–4: prove optimization correctness

```bash
python tools/verify_index_equivalence.py
```

Explain that every rule is executed twice over the same immutable events: once through the composite index and once through the unoptimized full-scan reference path. Semantic detections and execution traces must agree exactly after optimization-only metadata is removed.

Both paths share the evaluator, so this checks the indexing optimization; it does not independently establish that the rule is correct.

## Minute 4–5: show a valid negative result and the limits

```bash
soc-replay verify scenarios/benign-privileged-change
```

Show that the declared expectation is zero detections and that verification passes. This distinguishes a valid negative result from assuming that every successful demonstration must raise an alert.

Close with the boundaries:

- runtime state is deeply immutable;
- the rule language is deliberately small and inspectable;
- hashes provide internal integrity, not authorship or trusted time;
- benchmarks are environment-bound and remain outside deterministic bundle identity;
- live collection and infrastructure control remain outside the package; and
- the next meaningful proof is a measured physical experiment with containment and recovery evidence.

## Optional engineering checks

These checks extend the demonstration and need the development tools. They are also part of the [CI workflow](../.github/workflows/ci.yml).

```bash
python -m pip install -e ".[dev]"
python tools/validate_contracts.py
python tools/verify_deterministic_bundles.py
python tools/verify_reproducible_wheel.py
```

Real instances are checked against Draft 2020-12 schemas, scenario bundles are generated twice and compared byte for byte, and package construction is repeated under fixed build inputs.

## Adapter and benchmark extension

```bash
soc-replay normalize --adapter suricata-eve examples/adapters/suricata-eve.jsonl build/suricata-normalized.jsonl
python tools/benchmark_scenarios.py --copies 100 --iterations 15 --warmups 3
```

Use these commands to demonstrate the frozen offline adapter boundary and deterministic workload benchmarking with embedded equivalence proof. Timing measures this engine on the recorded host; it is not a production capacity claim.
