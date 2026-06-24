# Deep Modules

Design modules that provide meaningful behavior through small interfaces.

## Vocabulary

- **Module**: anything with an interface and implementation: function, class, package, feature slice, or tier-spanning unit of behavior.
- **Interface**: the full caller contract: signatures, invariants, ordering constraints, error modes, configuration, and performance.
- **Implementation**: the code and collaborators hidden behind the interface.
- **Seam**: where behavior can vary without editing callers.
- **Adapter**: a concrete implementation at a seam.
- **Depth**: leverage per unit of interface.
- **Locality**: change and verification stay concentrated.

## Signals

Deep module:

- Few entry points.
- Simple caller setup.
- Tests can prove behavior through the public interface.
- Implementation hides coordination, policy, and edge cases.
- Fixes concentrate in one module.

Shallow module:

- Interface nearly mirrors implementation.
- Callers must know sequencing, internal flags, or collaborator wiring.
- Tests mock through layers instead of proving behavior.
- Deleting the module removes little complexity.

## Design Questions

- Can one method replace several caller steps?
- Can parameters move from "caller must assemble" to "module can derive"?
- Can error modes and ordering rules live behind the interface?
- Does the seam represent real variation, or only speculative flexibility?
- Would this make tests more behavioral and less mock-heavy?
