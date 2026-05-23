> 🇬🇧 **English version:** [README_EN.md](./README_EN.md)

# Runtime Interoperability Contract

Minimal interoperability semantics for runtime observability and governance signals in AI systems.

---

## Objective

This repository explores minimal interoperability principles between:

- runtime observability layers;
- governance and orchestration systems;
- qualification or proof-anchoring layers.

The objective is not to create a global AI governance standard, but to define explicit, inspectable, and interpretable signals between independent layers.

---

## Principles

- Runtime observation remains distinct from proof anchoring.
- Governance authority remains distinct from measurement authority.
- Each layer must explicitly declare its observational scope and limits.
- Signals must remain usable by both machines and humans.
- Minimalism first, complexity later.

---

## Minimal Example of `measurement_surface`

```json
{
  "measurement_surface": {
    "visibility": "partial",
    "runtime_scope": "generation_stream",
    "observed_signals": [
      "G",
      "delta_G",
      "coherence"
    ],
    "limitations": [
      "no_access_to_model_weights",
      "no_internal_reasoning_visibility"
    ]
  }
}
