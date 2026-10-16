# silent-pass-catalogue

Each case here is a check that reported success after the property it was meant to establish had already failed, with a public reproducer and an upstream acknowledgement or an independent oracle.

[![links](https://github.com/repowazdogz-droid/silent-pass-catalogue/actions/workflows/links.yml/badge.svg)](https://github.com/repowazdogz-droid/silent-pass-catalogue/actions/workflows/links.yml)
[![release](https://img.shields.io/github/v/release/repowazdogz-droid/silent-pass-catalogue)](https://github.com/repowazdogz-droid/silent-pass-catalogue/releases)

## Try it

Case 9, about one second, Python standard library only:

```console
git clone https://github.com/repowazdogz-droid/omega-n1-permit-membership
cd omega-n1-permit-membership
python3 reproduce.py    # a permit that was never issued is accepted; exits 0 when both the flaw and the fix behave as documented
```

## What this does not show

- How often this happens. The cases come from one person's work and the projects that work
  touched, so no rate follows from them.
- That the fixes are complete. Each fix closes the instance listed, not the class.
- That the oracles are themselves free of this failure. Each is independent of the check it
  judges, which is all the admission rule asks.

## Cases

Admitted on: **oracle** (an independent reference decides the property without trusting the
check) or **upstream-confirmed** (a maintainer of the affected project acknowledged it). Cases
that miss the bar are in [APPENDIX_unconfirmed.md](APPENDIX_unconfirmed.md) with the reason.
Case 1 is a self-report about this account's own CI.

| # | case | domain | mechanism (why the check passed) | reproducer | admitted on | Status |
|---|---|---|---|---|---|---|
| 1 | `npm test \| tee` green on a failing suite | CI (GitHub Actions) | Steps ran under the implicit shell, `bash -e` without pipefail, so each step exited with `tee`'s status | [test/ci-can-fail.test.ts](https://github.com/repowazdogz-droid/mcp-authority-boundary/blob/main/test/ci-can-fail.test.ts) and the negative-control step in [verify.yml](https://github.com/repowazdogz-droid/mcp-authority-boundary/blob/main/.github/workflows/verify.yml) | oracle: a deliberately failing test run through the same pipe must turn the step red | Fixed in v1.1.1 (diff below) |
| 2 | Replay VERIFIED while a 100,000-byte write ran under a 4,096-byte cap | agent authorization (MCP, Cedar) | The resolver measured a one-element array argument as 0 bytes, the executor coerced it to a string, and the replay checked the record against itself | [run-all-local.sh](https://github.com/repowazdogz-droid/mcp-authority-boundary/blob/main/experiments/independent-observer/run-all-local.sh), [VERIFY.md](https://github.com/repowazdogz-droid/mcp-authority-boundary/blob/main/VERIFY.md) | oracle: an observer process that imports nothing from the system reads the written file | Fixed at `7b7e973` |
| 3 | Lean axiom gate that could not fire | proof checking (Lean 4) | `grep -qv` was inverted on a substring every output line contains, so `sorryAx` and a custom axiom passed | [harness/mutation_tests.sh](https://github.com/repowazdogz-droid/vsf-cjson/blob/main/harness/mutation_tests.sh), [CHANGELOG.md](https://github.com/repowazdogz-droid/vsf-cjson/blob/main/CHANGELOG.md) (B1) | oracle: the `#print axioms` set compared against an allow-list | Fixed in v1.0.1 |
| 4 | Lean file with no axiom output reported clean | proof checking (MCP wrapper) | A file that compiles but prints nothing for `#print axioms` gives a grep for disallowed axioms no lines to match | [tests/phase2_failure_modes.py](https://github.com/repowazdogz-droid/verify-mcp/blob/main/tests/phase2_failure_modes.py), fixture [Silent.lean](https://github.com/repowazdogz-droid/verify-mcp/blob/main/tests/fixtures/lean/Silent.lean) (L4) | oracle: the audit output the file must produce | Fixed: now reports `COULD_NOT_RUN` |
| 5 | Two eval logs with no aligned samples reported UNCHANGED, exit 0 | LLM evaluation (Inspect AI) | An empty set of aligned samples compared equal to itself, so a CI gate passed on a collapsed run | [tests/test_regressions.py](https://github.com/repowazdogz-droid/inspect-replay/blob/main/tests/test_regressions.py) | oracle: logs with injected, known changes | Fixed in 0.2.0 |
| 6 | loom reports `1 passed` on a build with a lost update | Rust concurrency (loom) | The atomic under test was not loom-instrumented, so loom never preempted at the racing access | [case-studies/03-loom-false-green-governor](https://github.com/repowazdogz-droid/evidence-audit/tree/main/case-studies/03-loom-false-green-governor) | oracle: the shimmed run finds the lost update | Detected: graded WEAK by evidence-audit |
| 7 | Completeness gate passed two judges whose 600 rows were all errors | LLM-as-judge pipeline | The gate counted rows, not usable verdicts | [test_gate_fires_on_the_real_pre_repair_state](https://github.com/repowazdogz-droid/evaltrust/blob/master/tests/test_agreement.py) on [verdicts.legacy.json](https://github.com/repowazdogz-droid/evaltrust/blob/master/data/agreement/verdicts.legacy.json) | oracle: usable-verdict count per judge | Fixed: gate counts usable verdicts |
| 8 | "Tampered chain is rejected" passed on every input | audit ledger | `verify_chain` read `result["ok"]`; the sealer returns `chain_intact`, and the missing key gave a falsy `None` | [test_verify_returns_ok_key_regardless_of_sealer](https://github.com/repowazdogz-droid/safeguards-control-plane/blob/master/tests/test_core.py), [LIMITATIONS.md](https://github.com/repowazdogz-droid/safeguards-control-plane/blob/master/LIMITATIONS.md) §10 | oracle: the intact chain must verify true and the tampered one false | Fixed |
| 9 | A permit that was never issued was accepted, and the repair ran | permission records | `validate_start` re-derived the reference number from the record's own body, so any self-consistent record validated | [reproduce.py](https://github.com/repowazdogz-droid/omega-n1-permit-membership/blob/master/reproduce.py) | oracle: membership in the issuing ledger | Fixed in `fix/`; original kept frozen |
| 10 | An LLM judge passed 4 of 6 decisions that violate an encoded property | agent governance | The judge read the decision text; the violation is a property of the encoded constraint | [records/omega](https://github.com/repowazdogz-droid/proof-carrying-evals/tree/master/records/omega) (pce-004, 007, 010, 011) | oracle: Z3 proves the violation in 6 of 6 | Measured result; no fix applies |
| 11 | `runpp_3ph` returns `converged=True` for Dy, Yy, Yd, Dd transformers | power-flow simulation (pandapower) | `pd2ppc_zero.py` skips zero-sequence modelling for those four groups before the `pf_3ph` gate that rejects the other unsupported groups | [e2nIEE/pandapower#3122](https://github.com/e2nIEE/pandapower/issues/3122) (reduced reproducer and an 18-group matrix) | upstream-confirmed: maintainer plans a regression test from the matrix (2026-10-06); no correctness oracle | Open upstream; PRs [#3145](https://github.com/e2nIEE/pandapower/pull/3145), [#3179](https://github.com/e2nIEE/pandapower/pull/3179) held for 4.0 |
| 12 | `CON_LECHE_INMODEL_CENSUS=1` exits 0 without running `checkDecls` | Lean tooling | The census mode returned before the check, contradicting the documented exit-code contract | [leanprover/con-leche#8](https://github.com/leanprover/con-leche/issues/8) | upstream-confirmed: "will update exit code" (collaborator), closed as completed 2026-09-10 | Fixed upstream |

## Case 1, self-report

The workflow had already fixed one instance of this class: run 34481249368 had masked a failed
`| tail -3`. Three other steps kept the same pipe-into-`tee` shape until an audit of the account
found them on 2026-10-16. Reproduced locally before the fix:

```
CI_NEGATIVE_CONTROL=1 bash -e -c 'npm run test:only 2>&1 | tee /tmp/old.log'           -> exit 0
CI_NEGATIVE_CONTROL=1 bash -eo pipefail -c 'npm run test:only 2>&1 | tee /tmp/old.log' -> exit 1
```

The fix, from `.github/workflows/verify.yml`:

```diff
+defaults:
+  run:
+    shell: bash
+
 jobs:
   implementation:
@@
           npm test 2>&1 | tee /tmp/npm-test.log
           echo "--- summary"; grep -E '^ℹ (tests|pass|fail)' /tmp/npm-test.log
+      - name: negative control (the npm test step above must be able to go red)
+        env:
+          CI_NEGATIVE_CONTROL: '1'
+        run: |
+          if npm run test:only 2>&1 | tee /tmp/negative-control.log; then
+            echo "FAIL: suite exited 0 with CI_NEGATIVE_CONTROL=1; a failing npm test cannot turn this job red"
+            exit 1
+          fi
+          grep -F 'deliberate failure: CI_NEGATIVE_CONTROL=1' /tmp/negative-control.log
+          grep -E '^ℹ fail 1$' /tmp/negative-control.log
```

The control is mutation-checked. With pipefail removed, the negative-control step exits 1.

## Submit a case

Open an issue with:

1. what the check reported, and the property it was meant to establish;
2. a public reproducer (a repository path, script or issue anyone can run);
3. either a link to an upstream maintainer acknowledging it, or the oracle that decides the
   property independently of the check.

A case with a reproducer but no acknowledgement or oracle goes in the appendix until it has one. Rates,
surveys and private reproducers are out of scope.
