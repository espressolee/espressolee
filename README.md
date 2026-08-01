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

**Elsewhere.** I sent [PyO3#5774](https://github.com/PyO3/pyo3/pull/5774),
a `Vec<u8>` fast path for u8-compatible buffer exporters. It was closed, and the
maintainer's reason was better than my patch: there's no guarantee the exporting
buffer is properly synchronized, so that choice belongs to user code rather than
to the extraction impl. CodSpeed also measured an 11.6% regression. Worth the
round trip.

Most of my work is in private repositories: a deterministic judgment kernel and
the audit methods around it. What I can show publicly is the discipline, not
the claims.
