# Diagram guidelines

Read this reference only when a diagram will help the reader inspect a result.
Keep shared handoff and fidelity rules in `SKILL.md`.

## Select the structure

Choose the structure that exposes the relationship needed for review:

| Subject | Useful structure |
| --- | --- |
| Component interactions or dependencies | A small relationship graph |
| Conditional execution or data movement | A flow with labeled branches |
| Ordered interactions between actors | A sequence diagram |
| State changes | States with labeled transition conditions |
| Evidence supporting a claim | An evidence map with explicit support or contradiction labels |

If the reader needs to compare attributes rather than follow relationships,
use a table. If text already makes the relationship clear, use text.

## Preserve edge meaning

Give arrows an explicit meaning. Use the source's actual direction and labels.
Distinguish calls, dependencies, data flow, state transitions, evidence support,
and causation. Label mixed edge types or explain a legend; proximity and arrow
style alone should not carry the distinction.

For each node or relationship, identify its supporting code, source statement,
observed output, or other available evidence. Preserve exact identifiers where
they matter. A conceptual grouping must not imply a real module or call that
the source does not establish.

For uncertain relationships:

- Include a supported inference only when useful, with an explicit label such
  as `inferred` or the equivalent in the user's language.
- Do not present uncertain relations as confirmed edges. A dashed edge alone
  is insufficient without an uncertainty label or legend.
- Omit unknown links rather than completing a plausible-looking flow.
- Show a local supported structure when the rest is unknown.

In causal explanations, distinguish observed order from demonstrated causation.
An evidence-support arrow is not a causal arrow. Preserve competing explanations
and relevant branch conditions when omitting them would change the conclusion.

## Keep the diagram focused

Include only the actors, states, and relationships needed for this review.
Make the reading direction clear. Use consistent labels and short branch
conditions. Explain abbreviations only when the reader needs them.

Put a short conclusion or explanation beside the diagram. Link evidence near
the relevant explanation instead of forcing long paths and logs into node labels.
Do not generate a whole-repository graph for a local issue.

## Match the host's capabilities

Use Mermaid only when host support is known or the user requests Mermaid source.
Choose a syntax supported by that host rather than assuming extensions work.

If rendering is unavailable or support is unknown, use an ASCII relationship
diagram, a small table, or prose. Respect an explicit plain-text preference.
When the user requests source, provide the requested source and explain it in
text; do not claim it rendered successfully.

## Check the actual result

Before delivery, check:

- Nodes and edges match the evidence; labels and branch conditions preserve meaning.
- Unknown or inferred links are omitted or explicitly qualified.
- Edge types and directions are clear; correlation is not promoted to causation.
- Important branches or competing explanations remain visible.
- Numbers, identifiers, and states agree with the accompanying explanation.
- The representation is readable with the host capabilities actually available.

If you can inspect only the diagram source, describe only source checks. Claim
visual verification only after rendering and inspecting the result. Inspect
overlap, clipping, label readability, and whether layout suggests unintended
relationships when visual tools are available. Do not invent a visual check
when those tools are unavailable.

Correct source or layout conflicts before delivery. If a diagram cannot preserve
the relationship clearly, use a simpler representation.
