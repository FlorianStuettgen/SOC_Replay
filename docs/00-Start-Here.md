# Start here

SOC_Replay demonstrates how an analytical result can carry enough evidence for another person to inspect and reproduce it. The working example is security telemetry: stored events are evaluated against explicit rules, then checked against declared expected outcomes.

## Two-minute review, without installation

1. Open the checked-in [reference report](../reference/network-scan/report.md). Seven synthetic events produce one high-severity detection backed by five named events and one simulated recommendation.
2. Read the decision summary and verification checks together. A PASS means the output matches the declared scenario; it does not establish real-world detection accuracy.
3. Follow the event IDs into the [source events](../scenarios/network-scan/events.jsonl) and inspect the [rule and expectations](../scenarios/network-scan/scenario.json).
4. Review the [implementation state](14-Implementation-State.md) for the boundary between implemented software, documented lab design, and future work.

For data and controls work, the transferable ideas are traceable inputs, explicit rules, repeatable checks, and preserved negative results. The current implementation is a security replay engine; cost, schedule, and commercial reporting would need their own domain models and validation.

## Technical review

SOC_Replay has two related surfaces:

```text
Stored synthetic or sanitized telemetry
  └─ SOC_Replay evidence engine
       ├─ validates exact scenario contracts
       ├─ replays detections deterministically
       ├─ compares indexed and full-scan execution
       └─ publishes verifiable evidence bundles

Segmented physical lab → documented research context
```

The Python package is an offline, simulation-only evidence engine and has no live infrastructure authority. The maintained demonstration uses synthetic fixtures; a complete measured physical-lab experiment is still pending.

## Continue into the implementation

1. Open the checked-in [reference report](../reference/network-scan/report.md).
2. Review the [implementation state](14-Implementation-State.md).
3. Read the [engineering review](16-Engineering-Review.md).
4. Run the [demo playbook](15-Demo-Playbook.md).
5. Check the [threat model](21-Threat-Model.md) before interpreting integrity claims.

## Evidence vocabulary

- **Exact verification** means output matches the scenario's declared detection contract.
- **Standalone bundle verification** means the bundle is internally consistent.
- **Source-bound reproduction** means the supplied scenario regenerates the committed artifacts byte for byte under the installed engine.
- **Differential correctness** means indexed execution matches the full-scan reference for tested inputs.
- **Reproducible build** means clean source copies produce identical wheels under the defined toolchain.

None of these alone establishes authorship, trusted time, independent custody, or production telemetry origin.

## Documentation numbering

The numeric filenames preserve the original platform documentation series. The goal-based map in [docs/README.md](README.md) selects the most relevant chapters for each review path; a chapter omitted from that map is still available in this directory.
