# openflexo-oslc — Backlog

Features and evolutions to develop. Each entry says what problem it solves and when it is done. Bugs that are known and not fixed go to `KNOWN_DEFECTS.md`, not here.

Identifiers are stable and never reused: `OSLC-F-<n>`. Reference them from commits and from other entries.

Status legend: `TODO` · `IN PROGRESS` · `BLOCKED` · `DONE` · `DEFERRED`.

---

## FML runtime errors

### OSLC-F-1 — The oslc adapter raises typed FML errors instead of returning null after a warning or a printed stack  ·  `DONE`

**GitHub:** [openflexo-team/openflexo-oslc#2](https://github.com/openflexo-team/openflexo-oslc/issues/2)

**Problem.** Part of `CORE-F-11` phase 6 (`openflexo-core/BACKLOG.md`), done with the other remaining adapters in one batch (asked by the user, 2026-11-16), with the rules settled on the first adapters (xlsx, xml, jdbc, rest, formod, docx): the edition actions caught the failures of their evaluations (four catches printing the stack, or a log), and went on with null after a warning.

**Rules applied (decided with the user on the first adapters).** Evaluations through `FMLEvaluation`; every `execute` / `performExecute` declares `throws FMLExecutionException`; a null receiver, model, document, name or required input is an `FMLNullReferenceError`; a select from a null source is an `FMLNullReferenceError` naming the select; a foreseen failure of the technology an `FMLTechnologyError` with a message of its own; a `ModelDefinitionException` an `FMLInternalError`; a cancelled loading an `FMLCancellation`; a typed failure of `AbstractCreateResource` rethrown unchanged; a log just before a throw removed (the failure is reported once, by the editor); the `System.out` traces of the actions removed. Scope: the paths that FML execution goes through; loading and conversion code left.

**Done (2026-11-16).**
- `AddOSLCRequirement` and the other actions: evaluations through `FMLEvaluation`, `execute` declares `throws FMLExecutionException`. The selects are stubs (their code is commented out): left.

**Known limits.**
- No test: oslc has no test module.

**After review (batch review).** `AddOSLCRequirement`: a null catalog is an `FMLNullReferenceError` (an NPE); its creation factory is
evaluated once.

**Verified (2026-11-16).** ECJ compiles oslc-ta (no test module).
