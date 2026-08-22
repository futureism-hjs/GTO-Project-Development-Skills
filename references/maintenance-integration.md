# Maintenance Integration

Use this document only for `/维护功能相关`. Read [Shared workflow](shared-workflow.md) for validation stages and [Evidence and validation map](verified-source-map.md) for exact maintenance patterns.

## Native Contract

Use the native maintenance system instead of creating a parallel fault system:

- choose a maintenance-aware controller class;
- add `Predicates.abilities(PartAbility.MAINTENANCE)` at an appropriate casing position;
- use `setExactLimit(1)` when exactly one maintenance hatch is required; use optional limits only when the product contract says maintenance is optional;
- keep tooltip claims consistent with the pattern and controller;
- verify that the attached part implements the maintenance interface and is collected by the controller;
- preserve original maintenance problems, recovery tools, perfect-maintenance behavior, and recipe penalties unless the request explicitly changes them.

The active one-stop rare-earth plant and many native electric/GCYM machines use exact-one patterns. Steam multiblocks should not gain maintenance by default; first select a controller that supports it.

## Validation

Validate the controller's maintenance collection, exact or optional limit, problem-state transitions, recovery behavior, recipe penalty, tooltip, pattern construction, and client/server loading. A maintenance predicate alone is not proof that the controller consumes the hatch.
