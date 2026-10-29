# Project Proposal: Model Discovery Agent for Reservoir History Matching and Forecasting

This project will investigate whether the Model Discovery Agent (MDA) framework proposed by Murphy (2026) can be adapted to discover reservoir-modelling workflows that produce accurate and well-calibrated forecasts after conditioning on available data.

Conventional history matching typically begins with a predefined geological model, simulator and inference method. Uncertain parameters are then adjusted to reproduce historical observations which may produce a good history match without producing reliable forecasts.

The MDA framework combines Bayesian inference, LLM proposals for new models and experiment design. This project will adapt MDA to propose and compare reservoir-modelling workflows, assessing forecast reliability, computational cost and the expert time required.

## Objective

A candidate workflow is represented as

$$
\mathcal{H} = (g, s, c, i, \psi),
$$

where:

- $g$ is a [geological model family](#geological-concepts-and-model-families)
- $s$ is a [simulator family](#simulator-families)
- $c$ defines the [coupling between geology and simulation](#geological-to-simulator-coupling)
- $i$ is the [inference or history-matching method](#inference-methods)
- $\psi$ is the [computational strategy](#computational-strategies) used to condition and evaluate the workflow

The overall objective is:

> **Discover workflows $\mathcal{H}$ that produce accurate, well-calibrated forecasts and stable decision-relevant conclusions within practical limits on computing cost and expert time.**

Geological knowledge will guide the search and candidate workflows must satisfy basic physical constraints and pass explicit checks against geological and physical evidence.

## Search space

### Geological concepts and model families

Geological concepts are plausible interpretations of how reservoir rocks are arranged and connected and are informed by seismic data, well logs, cores, regional geology and geological analogues. Several concepts may fit the same evidence.

A geological model family provides rules for generating realizations of a concept. A fluvial reservoir illustrates the distinction:

- **Geological concept:** Sandstone deposited in river channels, surrounded by lower-permeability floodplain mudstone.
- **Geological model family:** A set of rules for constructing sandstone channels as three-dimensional bodies, specifying how their geometry, placement, stacking, porosity and permeability can vary.
- **Realization:** One particular three-dimensional arrangement of channels and rock properties generated using those rules, potentially conditioned on well observations.

Realizations vary uncertain geometry and rock properties while retaining the family’s defining rules. Different modelling methods can represent the same geological concept.

### Simulator families

A simulator family is a class of dynamic reservoir models sharing governing physics, state variables, constitutive relationships and representations of wells, boundaries and unresolved processes.

Examples include:

- black-oil versus compositional formulations
- single-porosity versus dual-porosity flow

Comparing simulator families will test whether alternative representations of flow physics and unresolved processes improve forecast reliability or change decision rankings. Additional complexity will be justified by predictive benefit, rather than historical fit alone.

### Geological-to-simulator coupling

The coupling component specifies how a geological realization is translated into an executable simulation model. This may include upscaling a fine-resolution geological model onto a coarser simulation grid, assigning effective rock and flow properties, and representing the effects of faults on flow.

### Inference methods

The inference component defines how candidate models are conditioned on observations and how uncertainty is represented.

The project will remain open to established, adapted and newly developed approaches, for example:

- ensemble and iterative ensemble methods
- transport-map methods, including triangular transport

### Computational strategies

The computational component determines how simulation resources are allocated.

Examples include:

- reduced-order and surrogate models
- multilevel Monte Carlo (MLMC; Giles, 2015) and other multilevel or multifidelity methods

## Role of AI models in discovery and orchestration

### LLMs for hypothesis generation

Building on Murphy (2026), the project will use LLMs to propose and iteratively revise candidate workflow structures. One application will be to propose competing geological concepts and specify how they can be represented as geological model families. The focus is on changing modelling assumptions, not merely tuning parameters or generating additional realizations within a fixed family.

Proposals will be informed by documented geological knowledge, the current candidate library, and diagnostic summaries from conditioning and predictive checks. They may also suggest changes to simulator assumptions, geological-to-simulator coupling, inference methods and computational strategies.

Candidates will undergo geological, physical and implementation checks, with expert review where needed. 

### Decision models for orchestration

Alongside generative LLMs, the project may investigate learned decision models, such as Jev (TypeSafe, n.d.), for bounded decisions within the workflow. These models return typed choices or probabilities for predefined questions rather than generating hypotheses or code. Potential applications include routing diagnostic information to specialist processes, selecting among approved next-step tools, and flagging proposals that require further evidence or expert review. This orchestration role would extend beyond Murphy's hypothesis-expansion mechanism. 

Recent preprints motivate confidence-gated delegation (Wu & Lim, 2026; Zhu et al., 2026). Decision thresholds and fallback procedures will be tested on project-specific workflow runs, with uncertain or consequential cases escalated for further analysis or expert review.

## Forecast-oriented evaluation

Candidate workflows will be evaluated using for example:

- historical forecast tests at several cutoff dates, using only observations available at each date
- withheld wells, periods or observation types
- new observations collected after forecasts are made, where available

Evaluation should include both point and probabilistic forecast quality. Example measures include:

- a proper probabilistic score, such as log score or continuous ranked probability score;
- domain-specific forecast errors, such as pressure response, breakthrough time or cumulative production.

The framework should support retaining multiple workflows when available observations do not distinguish clearly among competing explanations.

### Guiding further data collection

The framework could also help identify which additional observations would be most useful for distinguishing among plausible workflows and improving decision-relevant forecasts. Candidate data-acquisition activities, such as pressure tests, tracer tests or time-lapse seismic, could be assessed across the retained workflows by considering possible outcomes and how they would update model support and forecasts. Expected information gain could guide observations aimed at resolving modelling uncertainty. Where reservoir-management decisions and their consequences are specified, expected value of information could be used to assess whether the anticipated benefit of better-informed decisions justifies the costs and delays of data acquisition.

### Connection to veridical data science

The project will use the predictability, computability and stability (PCS) framework of Yu and Kumbier (2020) to guide evaluation. For this reservoir application:

- **Predictability:** Assess forecast accuracy and uncertainty calibration on held-out observations.
- **Computability:** Measure the cost and scalability of conditioning, forecasting and repeated stability checks.
- **Stability:** Test the sensitivity of forecasts and decision rankings to appropriate changes in data preparation, observation-error assumptions, model families, inference methods, numerical resolution and random seeds.

Assumptions, perturbation choices, agent proposals, expert decisions and results should be documented so the analysis can be inspected and reproduced.

## References

- Giles, M. B. (2015). Multilevel Monte Carlo methods. *Acta Numerica, 24*, 259–328. https://doi.org/10.1017/S096249291500001X
- Murphy, K. (2026). *Model Discovery Agent: LLM-Assisted Bayesian Experiment Design for Data-Efficient Discovery of Mechanistic World Models*.
- TypeSafe. (n.d.). *System One*. Documentation. https://docs.typesafe.ai/concepts/system-one (accessed 29 October 2026).
- Wu, T., & Lim, W. Y. B. (2026). *REFLEX with Jev for Efficient Selective Control in LLM Agents*. arXiv preprint. https://doi.org/10.48550/arXiv.2609.26532
- Yu, B., & Kumbier, K. (2020). Veridical data science. *Proceedings of the National Academy of Sciences, 117*(8), 3920–3929. https://doi.org/10.1073/pnas.1901326117
- Zhu, K., Feng, T., Han, X., Wang, H., Yang, L., Tang, X., You, J., & Ji, H. (2026). *JevAgentHarness: Benchmarking, Deploying and Training Decision Models That Share Control with LLMs*. arXiv preprint. https://doi.org/10.48550/arXiv.2610.21412
