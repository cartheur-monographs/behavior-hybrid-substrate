# Experiment Procedure

This experiment README turns the paper's section 5 (`The GreenArrays Execution Path`) and section 6 (`Current Evidence Status`) into a concrete procedure.

The immediate goal is not yet a large multi-node behavior. The goal is to preserve the trusted `1737` `SOFTSIM` path, duplicate that stored block, make one tiny behavioral change, and verify whether the duplicated block still loads and steps. This is the smallest experiment that can show custom behavior in the current toolchain without mixing in unnecessary new failure modes.

## Experiment Objective

Determine whether the current blocker is specifically a block-context issue by testing whether a duplicated stored block derived from `1737` can be edited slightly and still execute through the same load path.

## Required Framing From The Paper

Section 5 implies a five-stage execution path:

1. start from a compact behavior description
2. keep node allocation explicit
3. implement per-node behavior in `arrayForth`
4. preserve declarative initialization and stored-block loading
5. use `SOFTSIM` as a functional witness, not as a timing-faithful hardware substitute

Section 6 adds the current evidence boundary:

1. `SOFTSIM LOAD` has already been observed to work
2. stock blocks `1698` and `1737` have already been observed to step with `SUPER`
3. prompt-entered mutations are not yet trusted because words such as `clc` and `unext` have failed in prompt re-entry
4. the next valid experiment must therefore preserve stored block context and mutate only one tiny part of the trusted baseline

## Specific Steps To Conduct

1. Start in the live `arrayForth` environment and use the installed project path that already reaches `SOFTSIM`.
2. Confirm the simulator vocabulary is available by running `SOFTSIM LOAD`.
3. Reconfirm the known-good baseline before attempting any edits.
4. Load stock block `1737`.
5. Step the simulator with `10 SUPER`.
6. Verify that stepping advances execution rather than leaving the display static.
7. Inspect block `1737` directly and treat it as the trusted baseline text.
8. Do not begin by retyping the block at the prompt.
9. Duplicate block `1737` into a fresh working block inside the live environment.
10. Verify that the duplicate matches the stored source before changing anything.
11. Identify the smallest possible edit inside `: go ...`.
12. Change only one tiny behavioral element in `: go`, and leave the rest of the block untouched.
13. Keep the same stored-block loading path used by the original block.
14. Load the duplicated block through that stored-block path rather than by prompt-entry reconstruction.
15. Step the duplicated block with `SUPER`.
16. Record whether it executes successfully and whether its behavior differs visibly from the original baseline.
17. If it fails, record the exact failure point, especially whether the failure again appears around block-context-sensitive words such as `clc` or `unext`.
18. Compare the result against the baseline to answer the central question: what does stored block execution provide that prompt re-entry does not?

## What To Record In Working Notes

Record the following for the run:

1. whether `SOFTSIM LOAD` succeeded
2. whether `1737 LOAD` succeeded
3. whether `10 SUPER` advanced the stock baseline
4. which block was used as the duplicate target
5. the exact one-line or one-word edit made inside `: go`
6. whether the duplicate loaded successfully
7. whether the duplicate stepped successfully
8. what visible difference, if any, appeared relative to the original
9. the exact point of failure if the duplicate did not run
10. the best current explanation of the block-context dependency

## Success Criterion

The experiment succeeds if a duplicated version of `1737`, changed only by one tiny edit in `: go`, still loads and steps through the stored-block path and shows a visible behavioral difference from the trusted baseline.

## Why This Experiment Comes First

This sequence follows the paper's discipline. First prove that the trusted small execution path is reproducible. Then prove that a custom one-node mutation is possible in that same context. Only after that should the project move on to a two-node cadence or supervisory handoff experiment.
