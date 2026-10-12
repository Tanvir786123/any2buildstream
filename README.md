# any2buildstream
**Investigating AI-assisted migration from existing build systems to BuildStream.**

## Overview
`any2buildstream` is an exploratory project investigating the use of AI agents to automate software build-system
migrations to [BuildStream](https://buildstream.build/).

The initial focus is migrating projects from Bazel to BuildStream. The longer-term objective is to investigate a
reusable approach that can be extended to other source build systems.

The project aims to determine how effectively AI agents can perform these migrations, how their work can be validated,
and where human engineering expertise remains necessary.

> **Project status:** Initial research and scaffolding. The migration
> workflow, validation tooling, and experimental methodology have not
> yet been implemented or evaluated.

## Motivation
Build-system migrations can require substantial engineering effort. They involve more than translating configuration
files: dependencies, toolchains, compiler and linker options, generated files, and build behaviour must also be
accounted for.

AI agents offer a potential way to streamline this process. However, generating a configuration that builds successfully
does not necessarily mean that the migration is correct.

This project investigates how AI agents can assist with build-system migrations while retaining a systematic means of
verifying their outputs.

The motivation is informed by [EngFlow's work on AI-assisted Bazel migrations](
    https://blog.engflow.com/2026/09/09/introducing-any2bazel-oss-bazel-migration-tool/), which explores the
    capabilities and limitations of AI agents when migrating existing projects to Bazel.

`any2buildstream` explores a related problem from the perspective of migrating existing projects to BuildStream.

## Research question
The primary research question is:
> How effective are AI agents at migrating software projects between
> build systems, and to what extent can this process be automated
> without human intervention while preserving build correctness and
> output equivalence?

The investigation will explore the following areas:
- **Migration correctness:** Can agents produce valid BuildStream configurations that preserve the intended build
behaviour?
- **Validation:** How can build-graph analysis and artifact comparison help establish whether a migration is correct?
- **Human intervention:** Where do agents require engineering assistance, and why?
- **Model capabilities:** Which capabilities and failure modes appear consistently across different frontier models?
- **Workflow effectiveness:** Can iterative, validation-guided migration reduce engineering effort compared with less
interactive approaches?

The aim is not simply to identify the best-performing model, but to understand the capabilities and limitations of
AI-assisted migration workflows.

## Initial scope
The initial scope is deliberately constrained to:

**Bazel → BuildStream**

This provides a defined starting point for investigating migration strategies, validation techniques, and agent
behaviour.

The initial work will focus on selecting suitable test projects, establishing reference builds, and investigating how
migration outputs can be validated systematically.

The longer-term direction is to explore a generalisable approach:
**Source build system → AI-assisted migration → BuildStream**

The initial implementation and its assumptions should not unnecessarily couple the overall architecture to Bazel.
However, supporting arbitrary source build systems is outside the initial scope.

## Proposed approach
The project will investigate a workflow consisting of the following stages.

### 1. Analyse the source project
Inspect the existing Bazel configuration to identify relevant build targets, dependencies, toolchains, compiler options,
generated files, and expected outputs.

### 2. Generate the BuildStream configuration
Provide an AI agent with a consistent environment, migration prompt, and set of available tools.

The agent will use this information to generate the BuildStream project configuration and required element files.

### 3. Build and validate
Evaluate the resulting migration using a combination of build execution, build-graph analysis, and artifact comparison.

Validation results should provide actionable feedback that can be used to investigate and correct migration failures.

### 4. Evaluate human intervention
Record where human assistance is required, the nature of the intervention, and its effect on the final result.

This will help distinguish migrations that can be completed autonomously from those that require engineering judgement.

### 5. Compare experimental results
Evaluate different models and agent configurations against consistent projects, prompts, environments, and acceptance
criteria.

The results will be used to investigate recurring capabilities, limitations, and failure modes.

## Validation
A central objective is to establish whether a migration is correct, rather than merely whether it builds.

The proposed validation strategy consists of several complementary approaches.

### Build success
Verify that the original Bazel project and migrated BuildStream project can build the required targets.

Build success is a necessary condition, but is not sufficient to establish migration correctness.

### Build-graph comparison
Investigate extracting build information from both systems and representing it in a common format.

A normalised representation could enable comparisons of relevant build actions, dependencies, inputs, outputs, and
command-line options.

The comparison must account for legitimate differences between build systems without hiding meaningful changes in build
behaviour.

### Artifact comparison
Investigate using [diffoscope](https://diffoscope.org/) to compare corresponding outputs produced by the original and
migrated builds.

This can help identify differences in executables, libraries, and other build artifacts.

Differences must be interpreted in context, accounting for factors such as toolchain versions, build configuration,
timestamps, and non-deterministic build steps.

### Functional testing
Where suitable tests exist, run them against both builds to provide additional evidence that the migrated project
retains its intended behaviour.

The final validation criteria and the interpretation of discrepancies remain to be defined.

## Experimental design
The investigation will aim to provide a consistent basis for comparing AI agents.

Areas to be established include:
- Selection of representative Bazel test projects.
- Reference BuildStream migrations for evaluating correctness.
- Selection of a small number of frontier models.
- A standardised migration prompt and agent environment.
- Consistent execution limits and available tooling.
- Repeatable experiments and structured result collection.
- Metrics for migration success, engineering effort, and human
  intervention.

The intention is to investigate capabilities and failure modes across different models, rather than to produce a simple
model ranking.

## Planned milestones
The following milestones describe the proposed direction of the project. They do not represent completed work.
- [ ] **Initial investigation:** Review existing migration tools, BuildStream capabilities, and related research.
- [ ] **Test projects:** Identify suitable Bazel projects and establish reproducible reference builds.
- [ ] **Migration workflow:** Define the initial agent environment, prompt, tools, and expected BuildStream outputs.
- [ ] **Prototype:** Implement an end-to-end Bazel-to-BuildStream migration workflow.
- [ ] **Validation:** Investigate build-graph extraction, graph comparison, artifact comparison, and functional testing.
- [ ] **Evaluation:** Run controlled experiments across selected models and test projects.
- [ ] **Analysis:** Document migration success rates, recurring failure modes, human interventions, and limitations.
- [ ] **Future direction:** Assess the feasibility of extending the approach to additional source build systems.

Milestones may be refined as the initial investigation establishes technical feasibility and identifies the most useful
areas of work.

## Longer-term direction
Although the initial focus is Bazel-to-BuildStream migration, the project aims to explore an approach that can be
extended to additional build systems.

Potential areas of future investigation include:
- Additional source build systems and language ecosystems.
- Reusable interfaces for extracting and comparing build information.
- Improved validation feedback for AI agents.
- More effective human-in-the-loop migration workflows.
- Evaluation against larger and more complex software projects.

Any extension beyond the initial scope will depend on the results of the initial prototype and evaluation.

## References
- [EngFlow: Introducing any2bazel, an OSS Bazel migration tool](
    https://blog.engflow.com/2026/09/09/introducing-any2bazel-oss-bazel-migration-tool/)
- [BuildStream documentation](https://docs.buildstream.build/)
- [diffoscope](https://diffoscope.org/)
- [BuildStream AOSP proof of concept](https://github.com/martinbrook/buildstream-aosp-poc)

## Status and contributions
This project is at an early exploratory stage. The proposed workflow, validation strategy, and experimental design are
subject to refinement as the investigation progresses.

Initial work will focus on establishing a tractable migration problem, identifying suitable reference projects, and
demonstrating a reproducible end-to-end workflow.
