# ReSTIR Foundations

This page covers the concepts that sit directly underneath ReSTIR. It assumes
basic path tracing intuition, but it avoids heavy derivations. The goal is to
know what each term is doing before reading the papers.

## Core Ideas

### Resampled Importance Sampling

Recommended resources:

- [Importance Resampling for Global Illumination](https://diglib.eg.org/items/7b8d7c38-ee96-4415-acdd-3dd164fa8fad), Talbot, Cline, and Egbert.
  - Core RIS paper for rendering; reading through Section 4.3 is enough for a first ReSTIR-oriented pass.
- [Importance Resampling for Global Illumination PDF](https://diglib.eg.org/server/api/core/bitstreams/0205c190-16aa-4f85-a26f-c7b3220683b9/content)
  - Direct PDF link for study notes.

Resampled Importance Sampling, or RIS, starts with a small set of candidate
samples, assigns each candidate a weight, and randomly selects one candidate
according to those weights.

Why it matters for ReSTIR: ReSTIR repeatedly performs this selection step, but
with candidates gathered from lights, neighboring pixels, and previous frames.

### Weighted Reservoir Sampling

Recommended resources:

- [Weighted Reservoir Sampling: Randomly Sampling Streams](https://link.springer.com/chapter/10.1007/978-1-4842-7185-8_22), Chris Wyman.
  - Practical introduction to weighted reservoirs and streaming samples.

Weighted Reservoir Sampling, or WRS, keeps one selected sample from a stream of
weighted candidates. It also tracks the accumulated weight needed to estimate the
final contribution.

Why it matters for ReSTIR: A reservoir lets the renderer combine many candidates
without storing all of them. This is essential for GPU-friendly real-time
rendering.

### Monte Carlo Estimators

A Monte Carlo estimator turns random samples into an approximate numerical
answer. In rendering, this answer is usually radiance or pixel color.

Why it matters for ReSTIR: ReSTIR changes how samples are selected and reused,
but the output still needs to be a valid estimator of lighting.

### MIS Balance Heuristic

The MIS balance heuristic gives each sampling strategy a weight based on how
likely that strategy was to generate the sample. It is one common way to combine
different estimators.

Why it matters for ReSTIR: ReSTIR papers often reason about samples that came
from different sources. MIS provides useful intuition for why contribution
weights must account for sampling probabilities.

### Path-Space Notation

Path-space notation describes a light transport path as a sequence of vertices:
camera point, surface interactions, light interactions, and connecting edges.

Why it matters for ReSTIR: Direct lighting can often be described with a simple
light sample. ReSTIR PT and GRIS need more general language for reusing entire
paths or transformed paths.

### Visibility And Geometry Terms

Visibility checks whether two points can see each other without occlusion. The
geometry term accounts for distance, angles, and measure conversion between
surfaces and directions.

Why it matters for ReSTIR: A reused sample may be valuable for one pixel but
invalid or weak for another. Visibility and geometry terms are part of deciding
whether reuse is useful.

### GPU-Friendly Path Tracing

GPU-friendly path tracing favors compact state, predictable memory access,
parallel work, and limited branching. Algorithms must fit within real-time frame
budgets.

Why it matters for ReSTIR: Reservoirs are attractive because they are compact.
They provide a practical way to increase effective sample count while keeping
memory and ray budgets manageable.

## Study Checkpoints

Before reading ReSTIR DI, try to explain:

- how RIS selects one sample from several weighted candidates,
- what information a reservoir stores,
- why a stream of candidates is useful on the GPU,
- why candidate weights depend on both contribution and sampling probability,
- why reusing samples can reduce noise but can also introduce bias or artifacts.

Before reading GRIS or ReSTIR PT, also review:

- how different sampling domains can describe related light paths,
- why a reused path may need a shift mapping,
- why unknown or hard-to-evaluate PDFs make reuse more subtle.

## Starter Resources

- [Importance Resampling for Global Illumination](https://diglib.eg.org/items/7b8d7c38-ee96-4415-acdd-3dd164fa8fad), Talbot, Cline, and Egbert.
  - Best source for RIS in rendering; read Sections 1-4.3 first.
- [Weighted Reservoir Sampling: Randomly Sampling Streams](https://link.springer.com/chapter/10.1007/978-1-4842-7185-8_22), Chris Wyman.
  - Best first resource for the reservoir data structure used by ReSTIR.
- [A Gentle Introduction to ReSTIR: Path Reuse in Real-time](https://intro-to-restir.cwyman.org/)
  - Best first ReSTIR-specific course after learning path tracing basics.
- [Spatiotemporal Reservoir Resampling for Real-Time Ray Tracing with Dynamic Direct Lighting](https://research.nvidia.com/publication/2020-07_spatiotemporal-reservoir-resampling-real-time-ray-tracing-dynamic-direct)
  - The original ReSTIR DI paper.
- [Generalized Resampled Importance Sampling: Foundations of ReSTIR](https://research.nvidia.com/labs/rtr/publication/lin2022generalized/)
  - The main theory bridge for GRIS and ReSTIR PT.
- [Physically Based Rendering: Sampling and Reconstruction](https://pbr-book.org/4ed/Sampling_and_Reconstruction)
  - General reference for sampling concepts used throughout rendering.

## Add Your Notes

Use this section to collect paper notes, short derivations, diagrams, and links.
Keep entries brief enough that the page remains a guide rather than a textbook.

Suggested format:

```md
- [Resource title](link)
  - Topic:
  - Why it helped:
  - ReSTIR connection:
```
