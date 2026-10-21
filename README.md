## espressolee

I work on CPython free-threading memory safety: finding C-extension lifetime
bugs, building controlled reproducers, and verifying fixes at exact revisions.

I build checks that can fail, and I try to keep what I claim under what I can
show. When I report a concurrency bug, I try to bring an exact revision, a
base-positive reproducer, negative and decoy controls, and an exact-head re-test
of the eventual fix.

### Accepted upstream

**Code I authored and maintainers merged:**

- [`cpython#159864`](https://github.com/python/cpython/issues/159864)
  → [`cpython#159884`](https://github.com/python/cpython/pull/159884) — reproduced
  a free-threaded `syslog.openlog()` crash when another thread changes
  `sys.argv[0]`, and fixed the borrowed reference. A core developer merged it
  into `main` on 2026-10-20; the 3.15 backport
  ([`#159952`](https://github.com/python/cpython/pull/159952)) is merged and the
  3.14 backport ([`#159954`](https://github.com/python/cpython/pull/159954)) is
  still open. Others had listed the problem before I reproduced it.
- [`zope.interface#380`](https://github.com/zopefoundation/zope.interface/issues/380)
  → [`zope.interface#382`](https://github.com/zopefoundation/zope.interface/pull/382)
  — contributed C cache-lifetime fixes and regression tests, revised with
  maintainers during review and merged on 2026-09-23.
- [`pybind11#6169`](https://github.com/pybind/pybind11/pull/6169) — added
  regression tests for syntax-error and traceback line numbers in evaluated
  Python source.
- [`Pillow#9953`](https://github.com/python-pillow/Pillow/pull/9953) — added
  regression tests for `ImageChops.offset()` on zero-sized images.
- [`python-rapidjson#236`](https://github.com/python-rapidjson/python-rapidjson/pull/236)
  — made the free-threading branch run CI, added a real 3.14t job with a
  fail-closed GIL assertion, and repaired the Python <3.13 collection failure.
- [`python-rapidjson#237`](https://github.com/python-rapidjson/python-rapidjson/pull/237)
  — restored `stubtest`, aligned constructor stubs with the C extension, and
  restricted wheel uploads to release tags.
- [`python-rapidjson#239`](https://github.com/python-rapidjson/python-rapidjson/pull/239)
  — fixed the rejected-argument allocation leak in `RawJSON()` and corrected
  its stub. It merged into `master` and shipped in
  [`v1.24`](https://github.com/python-rapidjson/python-rapidjson/tree/v1.24).
- [`nox#1153`](https://github.com/wntrblm/nox/pull/1153) — fixed tests that only
  passed when `uv` happened to be installed.
- [`mutmut#546`](https://github.com/boxed/mutmut/pull/546) — added ternary-condition
  mutation for a branch the mutation engine previously never touched.
- [`StringZilla#338`](https://github.com/ashvardanian/StringZilla/pull/338) — stopped
  the 5.x wheels from installing a top-level `test` package into `site-packages`;
  merged on 2026-10-02.

**Reports that led to upstream fixes:**

- [`Pillow#9892`](https://github.com/python-pillow/Pillow/issues/9892) →
  [`Pillow#9893`](https://github.com/python-pillow/Pillow/pull/9893) — reported
  out-of-bounds reads caused by sequence-length handling. Another contributor,
  lazerg, authored the fix; Pillow's maintainers merged it on 2026-09-25.
- [`pybind11#6180`](https://github.com/pybind/pybind11/pull/6180) — reported
  unconstructed sibling-base storage surviving deprecated constructor dispatch,
  and re-tested two reproducer paths against the maintainer's fix at exact revisions.
  The maintainer authored and merged the fix.
- [`CodeQL#22305`](https://github.com/github/codeql/issues/22305) →
  [`CodeQL#22310`](https://github.com/github/codeql/pull/22310) — reported lost
  Python taint flow through `list.extend`, `list.insert`, and `+=`; the merged
  maintainer/bot PR closes `extend` and `insert`. The larger `+=` gap is not
  claimed fixed.
- [`wrapt#347`](https://github.com/GrahamDumpleton/wrapt/issues/347) — reported a
  free-threaded double-free. The maintainer made a broader repair and released
  `2.4.0rc1`; I re-ran the same controlled harness at their request:
  **10/10 SIGSEGV → 0/10**.
- [`StringZilla#328`](https://github.com/ashvardanian/StringZilla/issues/328) —
  reported a stale-size borrowed-list-item crash; the maintainer fixed it in
  [`v5.1.1`](https://github.com/ashvardanian/StringZilla/releases/tag/v5.1.1).
- [`confluent-kafka#2319`](https://github.com/confluentinc/confluent-kafka-python/issues/2319)
  → [`confluent-kafka#2334`](https://github.com/confluentinc/confluent-kafka-python/pull/2334)
  — reported a borrowed-reference use-after-free in `Admin_create_topics`. A
  Confluent engineer wrote the fix, initially merged into the free-threading
  preview branch. On 2026-09-21, the maintainer
  [confirmed it had reached `master` and closed the issue](https://github.com/confluentinc/confluent-kafka-python/issues/2319#issuecomment-5759376877).
  I re-ran my harness against their exact fix head at their request.

**Merged fixes I reproduced or reviewed:**

- [`Pillow#9917`](https://github.com/python-pillow/Pillow/issues/9917) →
  [`Pillow#9919`](https://github.com/python-pillow/Pillow/pull/9919) — contributed
  additional re-entrant and concurrent mutation cases, then tested the
  Python-side copy at commit `655298e` on GIL and free-threaded builds. The
  report and fix PR were written by other contributors; Pillow's maintainers
  reviewed and merged it.

### Open work — not counted as accepted

Status in this section was rechecked on 2026-10-21.

- [`cpython#157841`](https://github.com/python/cpython/issues/157841) — reported a
  free-threaded 3.14 crash in concurrent `functools.partial.__setstate__`; open.

- [`pybind11#6177`](https://github.com/pybind/pybind11/pull/6177) — proposed
  removing an internals-lock acquisition from bound-call dispatch on free-threaded
  builds, with a regression test that checks whether dispatch can proceed while
  that lock is held. Open and not merged.
- [`python-rapidjson#235`](https://github.com/python-rapidjson/python-rapidjson/pull/235)
  — my free-threaded container-walk fix; open and not merged.
- [`tree#143`](https://github.com/google-deepmind/tree/issues/143) →
  [`tree#144`](https://github.com/google-deepmind/tree/pull/144) — reported and
  reproduced memory-safety faults in free-threaded dict traversal; the fix PR
  is open and was authored by another contributor.

### Closed contributions

- [`PyO3#5774`](https://github.com/PyO3/pyo3/pull/5774) — closed without merging
  after maintainer feedback. CodSpeed measured an 11.6% regression.
- [`Pillow#9854`](https://github.com/python-pillow/Pillow/pull/9854) — proposed a
  critical section around the `getlist()` walk for
  [`Pillow#9852`](https://github.com/python-pillow/Pillow/issues/9852); closed
  without merging. A maintainer closed the issue as completed on 2026-09-04.

### Things I made

- [firing-checks](https://github.com/espressolee/firing-checks) — includes negative
  controls and a `--selftest` that runs them for three of my own tools.
- [scanner-false-negatives](https://github.com/espressolee/scanner-false-negatives)
  — explored where a scanner's false negatives cluster; the experiments did
  not support the hypothesis. All 61 labels are public; they were LLM-produced,
  have no independent human regrade, and the repository is a post-hoc export.

### Limits

I am an external contributor, not a maintainer or core developer of the projects
above. Open PRs are not merged contributions, and unresolved reports are not
counted as fixes. Private reports are not public evidence and are deliberately
omitted. My own tools still have no independent adopter, so their value outside
my repositories remains unmeasured.

**Reach me:** espressolee1@gmail.com ·
[ORCID 0009-0003-0423-6686](https://orcid.org/0009-0003-0423-6686)
