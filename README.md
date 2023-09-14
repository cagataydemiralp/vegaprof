# VegaProf

This repository contains the source code for **VegaProf**, a performance profiler for Vega visualizations.
To learn more about our research behind VegaProf, check out our UIST'23 [paper](https://arxiv.org/pdf/2212.13670.pdf).
To see VegaProf in action, check out our [demo video](https://drive.google.com/file/d/1RblCu4f-cg2AUKqVSEMm4JtHhKg4DgDM/view?usp=sharing).
You can also try VegaProf [online](https://a13x.io/editor/#/edited), or clone this repo and follow the [usage instructions](#usage-instructions).

**Motivation**:
[Vega](https://github.com/vega/vega) is a popular domain-specific language (DSL) for visualization specification.
At runtime, Vega’s DSL is first transformed into a dataflow graph and then functions to render visualization primitives.
While the Vega abstraction of implementation details simplifies visualization creation, it also makes Vega visualizations challenging to debug and profile without adequate tools.

VegaProf effectively instruments the Vega library by associating the declarative specification with its compilation and execution.
As shown in Fig. 1, using interactive visualizations, VegaProf enables interactive visualization performance profiling at three abstraction levels: function, dataflow graph, and visualization specification.

![application](/misc/teaser_flat.png)

<figure>
  <figcaption align = "center">
    <b>
      Fig. 1 - VegaProf records low-level execution times and encodes them in coordinated visualizations corresponding to different abstraction levels of Vega’s DSL execution.
      (1) A flame graph provides an overview of where the major amount of time is spent.
      (2) In contrast to previous debugging approaches, which relied on trial and error-based changes of the visualization specification, VegaProf maps performance measures directly to the Vega specification.
      (3) To analyze performance on different levels, VegaProf augments the dataflow graph with performance measures.
    </b>
  </figcaption>
</figure>

<br />
<br />

## Overview

VegaProf consists of two main components, namely, a modified version of **Vega** that supports profiling measurements, and a visualization of these profiling results in the **Vega Editor**.

### Vega

![application](/misc/Mapping.png)

<figure>
  <figcaption align = "center">
    <b>
      Fig. 2 - During the lowering process, the DSL specification is parsed into a computation graph and then functions to be evaluated eventually.
      We hook into this lowering process and add annotations that indicate which element on a higher-level IR corresponds to what part of the lowered representation, e.g., what part of the spec corresponds to which data flow graph nodes.
      Based on these annotations, we can track measured function evaluation times back to higher levels, i.e., assigning time-measurements to the nodes of the dataflow graph and visualization specification.
    </b>
  </figcaption>
</figure>

<br />
<br />

We modified the implementation of Vega to record function execution times.
However, VegaProf does not only display timings on a function level, but maps these timings to the dataflow and specification level as well.
To support this, our implementation additionally tracks compile paths to resolve execution times on multiple levels as shown in Fig. 2.

### Vega Editor

![application](/misc/application.png)

<figure>
  <figcaption align = "center">
    <b>
      Fig. 3 - Our visual inspection tools were implemented in a familiar development environment – the Vega Editor.
      (A) We highlight selected regions of DSL code when inspecting performance bottlenecks.
      (B) A flame graph depicts the measurement of rendering function execution time.
      (C) In Vega’s dataflow graph, we highlight nodes that contribute to selected timing measurements.
    </b>
  </figcaption>
</figure>

<br />
<br />

As can be seen in Fig. 3, we augmented the Vega Editor with a performance tab to provide a performance profiler for Vega visualizations. To associate the declarative specification with its compilation and execution, we provide the folowing UI components and features:

- **editor view**:
  - _hovering/selecting_ a specification block
- **performance tab**:
  - **flame chart**: consists of stacks of rectangles, representing specification blocks and dataflow nodes
    - _hovering_ over a rectangle, to see the block ancestry and runtime
    - _clicking_ on a rectangle to zoom in
  - **dataflow graph**:
    - _zooming_
    - _hovering_ over a node, to see the parameters and generated values
    - _filtering_, by type or by the node execution time with the slider
  - **pulse table**:
    - _selecting_ a pulse to filter the nodes and see the corresponding flame chart
  - **node table**:
    - _hovering_ over the list of nodes ranked by execution time

## Usage Instructions

To run VegaProf locally, clone this repository and initialize its submodules:

```
git clone git@github.com:sigmacomputing/VegaProf.git
git submodule update --init
```

Next, build the modified version of Vega:

```
cd vega
yarn
yarn build
```

Then, you can install the dependencies of the Vega Editor and launch it via a local web server.

```
cd editor
yarn
yarn start
```

The local web server will be accessible from [http://localhost:8080](http://localhost:8080).

## Citing VegaProf

If you find this work or code useful please consider citing us:

    @article{yang2022vegaprof,
      title={VegaProf: Profiling Vega Visualizations},
      author={Yang, Junran and B{\"a}uerle, Alex and Moritz, Dominik and Demiralp, {\c{C}}a{\u{g}}atay},
      journal={arXiv preprint arXiv:2212.13670},
      year={2022}
    }
