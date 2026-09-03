## espressolee

I build checks that can fail, and I try to keep what I claim under what I can show.

Mostly CPython free-threading memory safety, C-extension lifetime bugs, and
tooling that refuses to pass quietly. When I report a concurrency bug, I try to
bring an exact revision, a base-positive reproducer, negative and decoy controls,
and an exact-head re-test of the eventual fix.

### Accepted upstream

**Code I authored and maintainers merged:**

- [`python-rapidjson#236`](https://github.com/python-rapidjson/python-rapidjson/pull/236)
  — made the free-threading branch run CI, added a real 3.14t job with a
  fail-closed GIL assertion, and repaired the Python <3.13 collection failure.
- [`python-rapidjson#237`](https://github.com/python-rapidjson/python-rapidjson/pull/237)
  — restored `stubtest`, aligned constructor stubs with the C extension, and
  restricted wheel uploads to release tags.
- [`nox#1153`](https://github.com/wntrblm/nox/pull/1153) — fixed tests that only
  passed when `uv` happened to be installed.
- [`mutmut#546`](https://github.com/boxed/mutmut/pull/546) — added ternary-condition
  mutation for a branch the mutation engine previously never touched.

**Reports that maintainers turned into fixes:**

- [`CodeQL#22305`](https://github.com/github/codeql/issues/22305) →
  [`CodeQL#22310`](https://github.com/github/codeql/pull/22310) — reported lost
  Python taint flow through `list.extend`, `list.insert`, and `+=`; the merged
  maintainer/bot PR closes `extend` and `insert`. The larger `+=` gap is not
  claimed fixed.
- [`wrapt#347`](https://github.com/GrahamDumpleton/wrapt/issues/347) — reported a
  free-threaded double-free. The maintainer made a broader repair and released
  `2.4.0rc1`; I re-ran the same controlled harness at their request:
  **10/10 SIGSEGV → 0/10**. The fix is theirs, not mine.
- [`StringZilla#328`](https://github.com/ashvardanian/StringZilla/issues/328) —
  reported a stale-size borrowed-list-item crash; the maintainer fixed it in
  [`v5.1.1`](https://github.com/ashvardanian/StringZilla/releases/tag/v5.1.1).
  Again: I found and reproduced it; they fixed it.
- [`confluent-kafka#2319`](https://github.com/confluentinc/confluent-kafka-python/issues/2319)
  → [`confluent-kafka#2334`](https://github.com/confluentinc/confluent-kafka-python/pull/2334)
  — reported a borrowed-reference use-after-free in `Admin_create_topics`. A
  Confluent engineer wrote the fix and it merged on 2026-09-02, but into the
  `dev_thread_free_support_preview` branch and not `master`; the issue is still
  open and nothing is released, so this is not a shipped fix. I re-ran my harness
  against their exact head at their request. The fix is theirs.

### Open work — not counted as accepted

Status in this section was rechecked on 2026-09-03.

- [`python-rapidjson#235`](https://github.com/python-rapidjson/python-rapidjson/pull/235)
  — my free-threaded container-walk fix; open, with its visible checks green at
  this refresh, but not merged.
- [`Pillow#9892`](https://github.com/python-pillow/Pillow/issues/9892) →
  [`Pillow#9893`](https://github.com/python-pillow/Pillow/pull/9893) and
  [`tree#143`](https://github.com/google-deepmind/tree/issues/143) →
  [`tree#144`](https://github.com/google-deepmind/tree/pull/144) — reproduced
  free-threading memory-safety faults with fix PRs now open. Those PRs are
  authored by their maintainers or other contributors, not by me.
- [`zope.interface#380`](https://github.com/zopefoundation/zope.interface/issues/380)
  — reproduced a free-threaded borrowed-cache lifetime fault; open, with no fix
  claimed here. A maintainer has since called its impact "theoretical at best",
  and I am not contesting that assessment.

### A negative receipt

- [`PyO3#5774`](https://github.com/PyO3/pyo3/pull/5774) — closed, and rightly.
  The maintainer's objection was better than my patch, and CodSpeed measured an
  11.6% regression. A record that shows only accepted work is not a record.

### Things I made

- [firing-checks](https://github.com/espressolee/firing-checks) — every check ships
  negative controls and a `--selftest` that runs them, because a guardrail nobody
  has seen fire is theater. It enforces that rule on three of my own tools.
- [scanner-false-negatives](https://github.com/espressolee/scanner-false-negatives)
  — I predicted where a scanner's false negatives cluster, and the data killed
  the claim twice. All 61 labels are public; they were LLM-produced, have no
  independent human regrade, and the repository is a post-hoc export.

### Limits

I am an external contributor, not a maintainer or core developer of the projects
above. Open PRs and reports are not accepted work. Private reports are not public
evidence and are deliberately omitted. My own tools still have no independent
adopter, so their value outside my repositories remains unmeasured.

**Reach me:** espressolee1@gmail.com ·
[ORCID 0009-0003-0423-6686](https://orcid.org/0009-0003-0423-6686)
