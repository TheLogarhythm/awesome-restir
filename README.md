# Awesome ReSTIR

A curated learning map for Reservoir-based Spatiotemporal Importance Resampling
(ReSTIR), focused on real-time ray tracing, path tracing, and modern sample reuse
research.

This repository is meant to be both:

- a beginner-friendly route into the ReSTIR literature, and
- a practical index of papers, courses, implementations, and math notes worth
  revisiting while studying.

## Start Here

ReSTIR is a family of rendering algorithms that reuse important light transport
samples across pixels and frames. The core idea is to keep a compact
**reservoir** of candidate samples, repeatedly resample it, and use nearby or
historical samples to raise the effective sample count without tracing many more
rays.

Before reading the ReSTIR papers, it helps to know the topics covered in
[notes/before-restir.md](notes/before-restir.md):

- the rendering equation and Monte Carlo integration,
- importance sampling and probability density functions,
- multiple importance sampling (MIS),
- path tracing and next-event estimation,
- temporal reprojection and basic real-time ray tracing constraints.

## Learning Roadmap

### 1. Foundations

Start with the short guide in [notes/foundations.md](notes/foundations.md). The
two most important statistical ideas are:

- **Resampled Importance Sampling (RIS)**: draw a small candidate set, weight the
  candidates by a target function, and resample one representative candidate.
- **Weighted Reservoir Sampling (WRS)**: maintain a streaming weighted sample so
  candidates can be combined without storing the whole candidate set.

Useful prerequisite topics:

- Monte Carlo estimators
- Importance sampling
- MIS balance heuristic
- Path-space notation
- Visibility terms and geometry terms
- GPU-friendly path tracing

### 2. ReSTIR Direct Illumination

Start with direct lighting. It is the cleanest version of the idea because the
sample domain is easier to understand than full paths.

- [Spatiotemporal Reservoir Resampling for Real-Time Ray Tracing with Dynamic Direct Lighting](https://research.nvidia.com/publication/2020-07_spatiotemporal-reservoir-resampling-real-time-ray-tracing-dynamic-direct), Bitterli et al., SIGGRAPH 2020.
  - The original ReSTIR paper.
  - Focus on candidate generation, reservoir updates, temporal reuse, spatial
    reuse, target functions, and the biased versus unbiased estimators.

### 3. ReSTIR GI and Volumes

After direct lighting, study how reservoirs are used for harder transport
problems.

- [ReSTIR GI: Path Resampling for Real-Time Path Tracing](https://research.nvidia.com/publication/2021-06_restir-gi-path-resampling-real-time-path-tracing), Ouyang et al., HPG 2021.
  - Extends reuse to multi-bounce indirect lighting paths.
  - Read this to understand why path reuse is powerful but harder than light
    sample reuse.
- [Fast Volume Rendering with Spatiotemporal Reservoir Resampling](https://graphics.cs.utah.edu/research/projects/volumetric-restir/), Lin et al., SIGGRAPH Asia 2021.
  - Applies reservoir resampling to participating media.
  - Useful once you are comfortable with transmittance, phase functions, and
    volumetric path sampling.

### 4. GRIS and ReSTIR PT

This is the theory bridge. Read this carefully after the direct-lighting paper
and the gentle course material.

- [Generalized Resampled Importance Sampling: Foundations of ReSTIR](https://research.nvidia.com/labs/rtr/publication/lin2022generalized/), Lin et al., SIGGRAPH 2022.
  - Introduces GRIS, a broader theory for correlated samples, unknown PDFs, and
    samples from different domains.
  - Also presents ReSTIR PT, a path-traced resampling algorithm.
  - Focus on shift mappings, Jacobians, unbiased contribution weights, and why
    reuse is no longer a simple independent-sample RIS problem.

### 5. Advanced Variants

Read these after GRIS. They are best treated as specialized branches rather than
the main entry path.

- [Conditional Resampled Importance Sampling and ReSTIR](https://research.nvidia.com/labs/rtr/publication/kettunen2023conditional/), Kettunen et al., 2023.
  - Studies conditional RIS and improves the theoretical framing of reuse.
- [Decorrelating ReSTIR Samplers via MCMC Mutations](https://research.nvidia.com/publication/2024-07_decorrelating-restir-samplers-mcmc-mutations), Sawhney et al., 2024.
  - Uses MCMC mutations to reduce correlation artifacts and reservoir sample
    impoverishment.
- [Area ReSTIR: Resampling for Real-Time Defocus and Antialiasing](https://research.nvidia.com/labs/rtr/publication/zhang2024area/), Zhang et al., SIGGRAPH 2024.
  - Extends reservoirs over film and lens dimensions for antialiasing and depth
    of field.
- [ReSTIR BDPT: Bidirectional ReSTIR Path Tracing with Caustics](https://research.nvidia.com/labs/rtr/publication/hedstrom2025restir/), Hedstrom et al., 2025.
  - Combines ReSTIR with bidirectional path tracing for caustics and
    hard-to-reach light paths.
- [ReSTIR PG: Path Guiding with Spatiotemporally Resampled Paths](https://research.nvidia.com/labs/rtr/publication/zeng2025restirpg/), Zeng et al., 2025.
  - Uses accepted ReSTIR paths to build guiding distributions for future
    candidates.
- [ReSTIR PT Enhanced: Algorithmic Advances for Faster and More Robust ReSTIR Path Tracing](https://research.nvidia.com/labs/rtr/publication/lin2026restirptenhanced/), Lin et al., 2026.
  - Practical engineering improvements for faster and more robust ReSTIR PT.
- [Stochastic Pairwise MIS for Unbiased Large-Kernel Reuse in Real Time](https://research.nvidia.com/labs/rtr/publication/hedstrom2026stochastic/), Hedstrom et al., 2026.
  - Enables unbiased reuse from many spatial neighbors in real time.
- [Gradient-Domain ReSTIR Path Tracing](https://research.nvidia.com/labs/rtr/publication/wang2026gradient/), Wang et al., 2026.
  - Combines ReSTIR-style reuse with gradient-domain path tracing.

## Courses and Talks

- [A Gentle Introduction to ReSTIR: Path Reuse in Real-time](https://intro-to-restir.cwyman.org/), SIGGRAPH 2023 course.
  - Best first educational resource after learning the basics of path tracing.
  - Use it before attempting GRIS in detail.
- [SIGGRAPH 2020 ReSTIR presentation](https://developer.nvidia.com/siggraph/2020/video/sig02-vid), NVIDIA Developer.
  - Presentation for the original direct-lighting work.

## Implementations and Frameworks

- [NVIDIA Falcor](https://github.com/NVIDIAGameWorks/Falcor)
  - Research rendering framework used by many NVIDIA real-time rendering papers.
- [RTXDI](https://developer.nvidia.com/rtxdi)
  - Production-oriented SDK for ReSTIR-style direct illumination.
- [RTX Path Tracing](https://github.com/NVIDIA-RTX/RTXPT)
  - NVIDIA path tracing sample framework.
- [DQLin/ReSTIR_PT](https://github.com/DQLin/ReSTIR_PT)
  - Reference-style ReSTIR PT implementation from Daqi Lin.
- [tatran5/Reservoir-Spatio-Temporal-Importance-Resampling-ReSTIR](https://github.com/tatran5/Reservoir-Spatio-Temporal-Importance-Resampling-ReSTIR)
  - Community ReSTIR implementation.

## Mathematical Reference

These are the ideas worth turning into short notes while learning.

### Target Function

The target function says how valuable a candidate sample is for a particular
receiver. In direct lighting, this usually includes emitted radiance, BSDF,
geometry, visibility, and the candidate sampling PDF.

Study goal: be able to explain what quantity the reservoir is trying to sample
from and how that differs from the proposal distribution.

### Reservoir Weight

A reservoir stores a chosen sample, a sum of candidate weights, and enough
metadata to estimate the contribution. The update rule is simple, but the
estimator becomes subtle once samples are reused across pixels or frames.

Study goal: derive the weighted reservoir update and explain why it can combine
streaming candidates.

### Unbiased Contribution Weights

Unbiased contribution weights, often abbreviated UCW, handle cases where reused
samples have unknown or hard-to-evaluate PDFs.

Study goal: understand why unknown PDFs appear in ReSTIR-style reuse and how
GRIS accounts for them.

### Shift Mappings

A shift mapping transforms a sample from one pixel, time, or path domain so it
can be evaluated at another receiver.

Examples include reconnection shifts, half-vector copy shifts, manifold shifts,
and specialized mappings for lens or subpixel dimensions.

Study goal: compare when a shift preserves useful paths, when it fails, and what
measure conversion is required.

### Jacobian Determinant

When a shift mapping changes variables, the estimator must account for the
change in measure. The Jacobian determinant is the conversion factor between the
old and new sample domains.

Study goal: explain why area measure, solid-angle measure, and path-space
measure cannot be mixed without conversion.

## Suggested Study Order

1. Review path tracing, importance sampling, and MIS.
2. Read the 2020 ReSTIR DI paper.
3. Work through the SIGGRAPH 2023 gentle introduction.
4. Implement or inspect a minimal direct-lighting reservoir.
5. Read ReSTIR GI to see how path reuse changes the problem.
6. Read GRIS slowly and write notes for every estimator term.
7. Study ReSTIR PT and compare it against ordinary path tracing.
8. Pick one advanced branch: antialiasing/DOF, caustics, path guiding,
   decorrelation, large-kernel reuse, or gradient-domain rendering.

## Contribution Style

For each new resource, prefer this shape:

```md
- [Title](link), Author et al., Venue Year.
  - Why it matters.
  - Best read after: prerequisite paper or concept.
  - Code/video/slides: links if available.
```

Keep the list curated. ReSTIR is a technical topic, so a smaller list with
reading guidance is more useful than a large unsorted bibliography.
