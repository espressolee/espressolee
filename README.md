## espressolee

I build checks that can fail, and I try to keep what I claim under what I can show.

Mostly CPython free-threading memory safety, and tooling that refuses to pass quietly.

**Receipts first — the externally-checkable parts, and their status:**

- [`mutmut#546`](https://github.com/boxed/mutmut/pull/546) · **merged** — a branch
  nothing in that mutation tester mutated.
- [`nox#1153`](https://github.com/wntrblm/nox/pull/1153) · **merged** — a test that
  only passed when `uv` happened to be installed.
- [`wrapt#347`](https://github.com/GrahamDumpleton/wrapt/issues/347) ·
  [`StringZilla#328`](https://github.com/ashvardanian/StringZilla/issues/328) ·
  **fixed upstream, released** — two reproduced free-threading crashes their
  maintainers fixed (`wrapt 2.4.0rc1`, which I re-measured at the maintainer's
  request: 10/10 SIGSEGV → 0/10; StringZilla `v5.1.1`). **Neither fix is mine.**
  I found, reproduced and re-verified; they fixed and released.
- [`Pillow#9854`](https://github.com/python-pillow/Pillow/pull/9854) ·
  [`python-rapidjson#235`](https://github.com/python-rapidjson/python-rapidjson/pull/235)
  · **open, unreviewed** — my own fixes. I deprioritized the Pillow one behind
  [#9853](https://github.com/python-pillow/Pillow/pull/9853), opened an hour
  earlier and more complete than mine.
- [`tree#143`](https://github.com/google-deepmind/tree/issues/143) ·
  [`confluent-kafka#2319`](https://github.com/confluentinc/confluent-kafka-python/issues/2319)
  · **open, no response** — reproduced use-after-frees, untouched.
- [`PyO3#5774`](https://github.com/PyO3/pyo3/pull/5774) · **closed, and rightly** —
  the maintainer's reason was better than my patch, and CodSpeed measured an 11.6%
  regression. It stays on this list: a record that shows only the accepted ones is
  not a record.

Six reproduced free-threading crashes, each linked above; a seventh is under
coordinated disclosure and is not described anywhere public. Finding them took a
survey of 224 packages, of which 65 had sites worth examining — but that write-up
is not public, and the per-package record behind the number is not published
anywhere, so the denominator is **stated, not checkable**. Six is a numerator
against a count you have to take my word for.

**Things I made:**

- [firing-checks](https://github.com/espressolee/firing-checks) — every check ships
  negative controls and a `--selftest` that runs them, because a guardrail nobody
  has seen fire is theater. It enforces that on three of my own tools. Nobody else
  uses it yet, so its value outside my own repositories is untested.
- [scanner-false-negatives](https://github.com/espressolee/scanner-false-negatives)
  — I predicted where a scanner's false negatives cluster, and the data killed the
  claim twice. All 61 labels are public. The labels were produced by an LLM, not by
  me reading 61 issues by hand; there is no independent human regrade; and the
  repository is a post-hoc export, so it cannot prove the ordering it describes.

**Limits:** no unrelated person has run any project of mine. Private work is not
evidence — judge the lines above, not what I say is behind the wall.

**Reach me** at espressolee1@gmail.com. GitHub shows a profile email only to
signed-in visitors, so it is repeated here where everyone can read it.
