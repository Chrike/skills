# Domain Modeling

Use this when architecture depends on precise domain language, or when the user explicitly asks to name concepts, define a glossary, or record an architectural decision.

## Language

- Prefer the project's existing domain terms when present.
- If a term is overloaded, ask which concept it names.
- If code and conversation disagree, surface the contradiction.
- Define domain terms by what they are, not by implementation details.

## Glossary Artifacts

Do not create or update glossary or domain-context artifacts during ordinary design work.

If the user asks to maintain a glossary, or a term is explicitly resolved and worth recording, use a compact glossary style:

```md
# <Context Name>

## Language

**Order**:
One or two sentences defining the concept.
_Avoid_: Purchase, transaction
```

Only include project-specific domain concepts, not general programming terms.

## ADRs

Offer an ADR only when all are true:

- The decision is hard to reverse.
- A future reader would find it surprising without context.
- Real alternatives were considered.

Keep ADRs short: title plus 1-3 sentences is often enough. Do not create ADRs without user agreement.
