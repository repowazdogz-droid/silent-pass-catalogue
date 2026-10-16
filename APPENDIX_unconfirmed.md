# Appendix: cases not in the main table

Each line gives the reason the case is not in [README.md](README.md). Two kinds of entry:

- **QUALIFIES (held):** meets the admission bar (a public reproducer, plus an oracle or upstream
  acknowledgement) but was held out. The main table keeps one case per mechanism, and each held
  entry names the case it duplicates or the reason it was set aside.
- **EXCLUDED:** fails the bar, or is not an instance of a check reporting success after its
  property failed.

| case | repo / venue | verdict | reason |
|---|---|---|---|
| "Effect verified" while every stage-4 check was record-consistency (claim D) | mcp-authority-boundary | QUALIFIES (held) | Same mechanism as case 2: the record was checked against itself. |
| `sorry` gate never fired (Lean prints it in backticks) | vsf-cjson | QUALIFIES (held) | Same harness and mechanism family as case 3. |
| Stale `.olean` let a mutated proof pass the axiom check | vsf-cjson | QUALIFIES (held) | Same harness as case 3. |
| PORT_WRONG = 0 inferred from a counter nothing emitted | vsf-cjson | QUALIFIES (held) | A distinct mechanism (an assigned verdict, not a computed one); held only to keep one case per repo. |
| Rocq/TLC wrapper defects 1, 4, 5 and 6 | verify-mcp | QUALIFIES (held) | Same repo and wrapper layer as case 4. |
| Kani assertion counted while UNREACHABLE | evidence-audit | QUALIFIES (held) | Same repo as case 6. |
| `#print axioms` identical for a correct and a wrong-valued model | evidence-audit | QUALIFIES (held) | Same repo as case 6. |
| Build green with a false axiom (`canonicalBytes_injective`) | omega-lean-proof | QUALIFIES (held) | The axiom sat in the trusted base, which is a different claim from a check passing; held pending a clean statement of which check is meant. |
| Regex and substring judges pass plausible-but-wrong answers | evaltrust | QUALIFIES (held) | Same shape as case 10 (a soft judge passes a violation). |
| `check_tla.sh` exits 0 when no JDK is present | capctl-iris | QUALIFIES (held) | Same mechanism as case 1 (exit status detached from the work). |
| Mutants M1 and M5 pass every property written from the specification | spcu-verification | QUALIFIES (held) | Specification inadequacy rather than a broken check; it belongs in a separate class. |
| Hash-chain check passes a re-sealed forgery | nanogpt-provenance | QUALIFIES (held) | A constructed demonstration, not an occurrence found in use. |
| Single-writer mode reports 100% enforcement while 6 unsafe actions ran | safeguards-control-plane | QUALIFIES (held) | A designed demonstration of the phenomenon, not a defect found. |
| Every Cedar request ALLOW while the weights are exfiltrated; backdoor keeps 0.943 clean accuracy | ai-research-security-plane | QUALIFIES (held) | Constructed demonstrations, and the headline numbers are not asserted in CI. |
| cJSON accepts `"\uZZZZ"` as `""` | vsf-cjson (target: DaveGamble/cJSON) | QUALIFIES (held) | Not yet reported to the cJSON maintainer; listing it publicly before that report would be the wrong order. |
| Arena test reported `ok` with zero cases run when `LEAN_KERNEL_ARENA` is unset | omega-checker | EXCLUDED for now | Fix and test are in an open PR, so the reproducer is not yet on the public default branch. |
| Negative control counted any compile error as a rejection | escrow-budget-mpst | EXCLUDED for now | Fix and meta-control are in an open PR, so not yet on the public default branch. |
| `model_graded_qa` with `scored_samples=0` still shows a metric row | inspect-audit / UKGovernmentBEIS/inspect_ai#4286 | EXCLUDED | Upstream acknowledgement not checked, and the oracle is partial (it judges the denominator, not grade correctness). |
| Guardrail failed 100% of calls and reported 0 blocks | commons-agent-lab | EXCLUDED | The public reproducer is recorded results; a re-run needs a paid API key, so no runnable reproducer. |
| Apalache reports NoError when `--init` is silently ignored | escrow-budget | EXCLUDED | Documented in prose only (`FLOORD-CTI.md`); no runnable reproducer and not reported upstream. |
| Mutation helper reported "No error" without mutating | escrow-budget | EXCLUDED | The helper script is not in the repository. |
| `p1_state_legal` and `p1_pstate_legal` cannot fail | spcu-verification | EXCLUDED | The assertions are vacuous, but no property is shown to have failed. |
| Byte-identical sealed records still assert a withdrawn claim | collective-bound | EXCLUDED | Byte reproduction never claimed to establish the claim's truth, so this is not a failed check. |
| Record says "QUARANTINED, NOT READ" after the file was read (F-9) | omega-openadmet-cyp-challenge | EXCLUDED | The referee classed it as a wording defect; no leakage occurred, so no property failed. |
| Floor check scored cells escapable on sighting while possession was unreachable (ID3) | one-world | EXCLUDED | Described in `DEVIATIONS_EXP3.md` only; no runnable reproducer, and still open. |
| Supplied residual 6.76e-8 while the independent inventory drifts 6.52e-3 mol | pybamm-5700-lithium-inventory | EXCLUDED | Unconfirmed: the repository never frames the supplied expression as a check that passed. |
| Lexical validator scores a paraphrased judgement as clean | case-notes-eval | EXCLUDED | No oracle: whether the paraphrase still conveys the judgement is never computed. |
| `completed_samples` depended on scorer declaration order | UKGovernmentBEIS/inspect_ai#4602 | EXCLUDED | Fixed upstream (PR #4604), but it is a wrong count, not a success report after a failure. |
