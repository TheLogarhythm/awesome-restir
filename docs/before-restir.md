# Before Reading ReSTIR Papers

This page lists the rendering ideas that make ReSTIR papers much easier to
read. You do not need to master every topic first. Aim for enough intuition to
understand what problem ReSTIR is trying to solve.

## What To Learn

### [The Rendering Equation](https://dl.acm.org/doi/10.1145/15886.15902)

Recommended reading: [PBRT v4: The Light Transport Equation](https://pbr-book.org/4ed/Light_Transport_I_Surface_Reflection/The_Light_Transport_Equation)

The rendering equation describes outgoing light at a surface as emitted light
plus reflected incoming light from many directions. In practice, it explains why
rendering is an integration problem: a pixel depends on many possible light
directions and paths.

Why it matters for ReSTIR: ReSTIR improves how we choose and reuse samples for
this integral. Without the rendering equation, the reservoir can feel detached
from the image it estimates.

### Monte Carlo Integration

Recommended reading: [PBRT v4: Monte Carlo Integration](https://pbr-book.org/4ed/Monte_Carlo_Integration)

Monte Carlo integration estimates a difficult integral with random samples.
Rendering uses it because the full set of light paths is too large to evaluate
directly.

Why it matters for ReSTIR: ReSTIR is a sampling algorithm. It does not remove
Monte Carlo estimation; it tries to make each sample more useful.

### Importance Sampling And PDFs

Recommended introductory YouTube video: [Importance Sampling](https://youtu.be/C3p2wI4RAi8?si=ppts6TSnLi3-EzAG)

Importance sampling chooses samples from distributions that are likely to
contribute more to the result. A probability density function, or PDF, describes
how likely a sample is under a sampling strategy.

Why it matters for ReSTIR: ReSTIR compares and combines candidates using weights.
Those weights only make sense if you understand that estimators compensate for
how samples were generated.

### Multiple Importance Sampling

Recommended YouTube videos: [Multiple Importance Sampling](https://youtu.be/2S6imDIiFTM?si=tnRdkZ9fcCUOacKm), [Continuous Multiple Importance Sampling (SIGGRAPH 2020 Presentation)](https://youtu.be/dxFSwplfdpk?si=Qdn7x4HaFIDq4Mur)

Multiple importance sampling, or MIS, combines samples from different strategies
while reducing variance. In rendering, common strategies include sampling a
light source or sampling a BSDF direction.

Why it matters for ReSTIR: ReSTIR often mixes candidates from different pixels,
frames, or domains. MIS intuition helps explain why weighting and balance matter.

### Path Tracing And Next-Event Estimation

Path tracing estimates light transport by tracing random paths through the
scene. Next-event estimation directly samples light sources at a surface point,
which is especially important for direct lighting.

Why it matters for ReSTIR: ReSTIR DI starts from direct-lighting samples, while
ReSTIR GI and ReSTIR PT extend the idea to paths and indirect lighting.

### Temporal Reprojection

Temporal reprojection maps information from a previous frame to the current
frame using motion, depth, and surface information. It is widely used in
real-time rendering to reuse work over time.

Why it matters for ReSTIR: Temporal reuse is one of ReSTIR's central strengths.
The algorithm can reuse candidate samples from earlier frames when they are
still valid for the current frame.

### Real-Time Ray Tracing Constraints

Real-time rendering has tight frame budgets, limited rays per pixel, and GPU
memory constraints. Algorithms must be parallel, stable under motion, and cheap
enough to run every frame.

Why it matters for ReSTIR: ReSTIR is designed for situations where tracing many
independent samples per pixel is too expensive.

## Study Checkpoints

You are ready to start the original ReSTIR paper when you can explain:

- why rendering is usually estimated with random samples,
- why sample PDFs appear in estimators,
- why direct light sampling can be noisy with many lights,
- what it means to reuse data from a previous frame,
- why real-time rendering cares about getting more value from fewer rays.

## Starter Resources

- [How Ray Tracing (Modern CGI) Works And How To Do It 600x Faster](https://youtu.be/gsZiJeaMO48?si=XdMNVhKHoliSgz7w)
  - Friendly visual introduction to ray tracing and why ReSTIR is useful.
- [Ray Tracing in One Weekend](https://raytracing.github.io/)
  - Practical first exposure to ray tracing and path tracing.
- [Physically Based Rendering: From Theory to Implementation](https://pbr-book.org/)
  - Deeper reference for rendering theory, Monte Carlo integration, sampling,
    and light transport.
- [Scratchapixel: Monte Carlo Methods in Practice](https://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/monte-carlo-methods-in-practice/monte-carlo-integration.html)
  - Beginner-friendly explanation of Monte Carlo integration.
- [Scratchapixel: Global Illumination and Path Tracing](https://www.scratchapixel.com/lessons/3d-basic-rendering/global-illumination-path-tracing/global-illumination-path-tracing-practical-implementation.html)
  - Practical introduction to path tracing and rendering-equation intuition.

## Add Your Notes

Use this section to collect resources, questions, and short explanations as you
study. Good additions include one-sentence takeaways, links to chapters, and
small examples that clarify a concept.

Suggested format:

```md
- [Resource title](link)
  - Topic:
  - Why it helped:
  - ReSTIR connection:
```
