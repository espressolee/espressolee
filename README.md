## espressolee

I build checks that can fail, and I try to keep what I claim under what I can show.

That sounds like a slogan, so here is what it costs. The robot below won its
contest and I published the source unmodified, with the bug still in it and a
note on why it stays. The audit pipeline in my own public repo turned out to
emit hardcoded scores — `composite: 8.2`, `recommendation: ACCEPT`, computed
from nothing — so I deleted it instead of repairing it, because repairing it
would have turned a red X into a green badge certifying invented numbers.

---

**[firing-checks](https://github.com/espressolee/firing-checks)** ·
Small checks that prove they can fail.

A guardrail nobody has seen fire is theater. The usual failure isn't a missing
test — it's a check that reports clean because it silently *couldn't run*: a
parser that returned an empty list, a subprocess whose exit code got dropped, a
predicate never reached. Each tool ships negative controls that construct the
violation and assert rejection; `--selftest` runs them. Three tools, and the
third enforces that contract over the other two — and over itself, registered in
its own registry with no exemption for the enforcer. It counts the control lines
a run printed rather than trusting the summary a script writes about itself. No
dependencies, Python 3.10+, each file standalone.

**[scanner-false-negatives](https://github.com/espressolee/scanner-false-negatives)** ·
A pre-registered study that refuted its own hypothesis, twice.

I built a scanner, hypothesized where its false negatives cluster, and designed
the test so it could kill the claim rather than confirm it. It did — the naive
version on Bandit, and the successor on semgrep, using a discipline rating of
each layer sealed *before* any defect was classified, so the concordance could
not be circular. All 61 labels are published so a stranger can re-grade them.
Exploratory: one grader, no inter-rater agreement yet, and the topic is not new —
the limits and prior art are on the page.

**[WarmLogic-OSS](https://github.com/espressolee/WarmLogic-OSS)** ·
Evidence-centric governance tooling — public subset.

An evaluation harness where a run produces a manifest, a decision log, and a
verify report, and verification refuses a run whose stored verdict its own event
log does not support. Toy workloads; the refusal is real and one shipped run
triggers it.

**[xinrui-robot-2023](https://github.com/espressolee/xinrui-robot-2023)** ·
1st place, 7th Xinrui Innovation Robotics Contest — HIT, May 2023.

Team 21, out of 20 teams and ~90 participants. Freshman year, four of us from
four countries. Two Arduino boards split by concern, joined over serial. The source
is published exactly as it ran, including a bug, with the reasoning for leaving
it there. [Match report](https://today.hit.edu.cn/article/2023/05/17/103860)
(university, third-party record).

---

**Elsewhere.** Three patches to projects I do not own: **two merged, one closed** —
and the closed one's maintainer had a better reason than my patch.

[mutmut#546](https://github.com/boxed/mutmut/pull/546) — **merged.** A ternary is
a branch nothing in that mutation tester mutated, and branch coverage cannot see
an uncovered arm either, so an untested arm read as a pass twice over. Two mutants
per ternary now force it down each side. The review on an earlier attempt had
asked to drop the precedence wrapping; that is right for `or True` and wrong for
`and False`, because `and` binds tighter than a top-level `or` — over all 16
assignments of `a if b or c else d`, the unwrapped form differs from the original
on 2 of 16 rather than 6, and only when `b` is falsy, i.e. a mutant tests mostly
cannot kill. I brought the table rather than the opinion. Closes their #196.

[nox#1153](https://github.com/wntrblm/nox/pull/1153) — **merged.**
Their uv download tests failed on a machine without uv installed. +15/-0.

[PyO3#5774](https://github.com/PyO3/pyo3/pull/5774) — **closed, and rightly.** A
`Vec<u8>` fast path for u8-compatible buffer exporters. The maintainer's reason
was better than my patch: there is no guarantee the exporting buffer is properly
synchronized, so that choice belongs to user code rather than to the extraction
impl. CodSpeed also measured an 11.6% regression. Worth the round trip, and it
stays on this list — a record that only shows the accepted ones is not a record.

**Free-threading (no-GIL) memory safety.** Two use-after-free reports in C
extensions, each with a minimal reproducer and three controls — no-mutator,
decoy, and GIL-on — so the crash is shown to be a free-threading fault, not a
generic race.

[google-deepmind/tree#143](https://github.com/google-deepmind/tree/issues/143) —
a borrowed dict key from `PyDict_Next` is dereferenced across `__hash__`/`__eq__`
in `assert_same_structure`; 10/10 SIGSEGV, and **live by default** because
pybind11 declares the module no-GIL-safe when it is not.

[confluentinc/confluent-kafka-python#2319](https://github.com/confluentinc/confluent-kafka-python/issues/2319)
— a borrowed `NewTopic` list item is cast to a C struct and its fields read
against a stale count; 10/10 SIGSEGV with the GIL forced off, and **latent**
until the module opts into free-threading — the GIL-on control stays clean, and
the report leads with that rather than overstating. The scanner that surfaced
these is a static tool I wrote; a general release is deliberately withheld.

Most of my work is in private repositories, and it stays private — so I will not
dress it up as an accomplishment here. A claim you cannot check is exactly the
thing this profile is about not making. Judge me on the public evidence: the
reproducers, the negative controls, and the labels published for re-grading.

**Reach me** at espressolee1@gmail.com. It is in the sidebar too, but GitHub
shows a profile email only to signed-in visitors, so it is repeated here where
everyone can read it.
