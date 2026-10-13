# Dhaka Urban Road Network Dataset

**A directed urban road-network dataset for transportation research in Dhaka, Bangladesh**

[![Repository](https://img.shields.io/badge/Repository-GitHub-black?logo=github)](https://github.com/souravdebnath109/dhaka-urban-road-network-dataset)

## 1. Overview

The **Dhaka Urban Road Network Dataset** is an integrated, attributed representation of the urban road network of Dhaka, Bangladesh, developed to support research in transportation-network analysis, heterogeneous traffic routing, congestion-aware path planning, and urban mobility optimization.

The dataset was developed by combining a manually digitized road network covering central Dhaka with an extended road network derived from OpenStreetMap (OSM). The resulting network represents intersections or network locations as nodes and road segments as directed links.

The dataset was developed for a research study on **road-condition-aware heterogeneous traffic routing using a modified Ant Colony Optimization (ACO) algorithm** on a large-scale urban network.

It is intended to support reproducible transportation experiments, algorithm benchmarking, graph-based network analysis, and further research on vehicle-aware route planning.

### Dataset summary

| Property | Description |
|---|---|
| Dataset name | Dhaka Urban Road Network Dataset |
| Geographic area | Dhaka, Bangladesh |
| Network representation | Directed graph |
| Number of nodes | 57,424 |
| Number of directed links | 151,886 |
| Road hierarchy | Arterial, collector, and local |
| Main data sources | Manually digitized road network and OpenStreetMap |
| Principal application | Urban traffic routing and transportation-network optimization |
| Routing workload in the associated study | 1,450 synthetic routing requests |
| Vehicle classes in the associated study | 5 |
| Intended users | Transportation researchers, network scientists, and optimization researchers |

The node and link counts describe the network reported in the associated research manuscript. Users should verify that the files released in this repository correspond to the same network version.

## 2. Research motivation

Urban road networks contain roads with different functional roles, geometries, capacities, and accessibility constraints. These differences influence the suitability of individual roads for different vehicle classes and the distribution of traffic across the network.

This dataset provides a graph-based representation suitable for investigating questions such as:

- How can large urban road networks be represented for computational routing experiments?
- How can heterogeneous vehicle classes be accommodated in route planning?
- How can road hierarchy and vehicle–road compatibility be incorporated into routing algorithms?
- How does coordinated routing influence network congestion and spatial load distribution?
- How do optimization methods compare with conventional shortest-path routing?
- How do network structure, route feasibility, and search restrictions affect computational performance?

The dataset is intended as a research resource rather than a claim that every road attribute represents a directly observed, time-varying traffic condition.

## 3. Geographic coverage and network construction

The network was constructed from two complementary components.

### 3.1 Manually digitized central-Dhaka network

The central-Dhaka component was prepared through QGIS-assisted mapping and visual inspection of road information.

Major intersections or network locations were represented as nodes, and road segments between consecutive intersections were represented as directed links.

The manually prepared component was checked using publicly observable map and street-level information for road classification and lane-count information.

### 3.2 OpenStreetMap-derived road network

Additional road coverage was obtained from OpenStreetMap through the Overpass API.

Available OSM road attributes were retained where applicable. Missing lane information in the OSM-derived component was assigned using the highway-class defaults adopted in the associated study.

### 3.3 Network integration

The manually digitized and OSM-derived components were integrated into a single directed graph.

The resulting network contains:

- **57,424 nodes**
- **151,886 directed links**

The integrated representation supports network connectivity analysis and the construction of attributed routing graphs.

The two source components should be distinguished when interpreting the data. Their attributes may differ in provenance, completeness, and level of verification.

**Data provenance note:** The network is a research-prepared integration of multiple sources. It should not be interpreted as an official, exhaustive, or continuously updated representation of every road in Dhaka.

## 4. Road-network representation

The network is represented as a directed graph:

\[
G=(V,E),
\]

where:

- \(V\) denotes the set of network nodes.
- \(E\) denotes the set of directed road links.

Each directed link connects an origin node to a destination node. Where both directions of travel are represented, they should be stored as separate directed links when the underlying road data supports that representation.

### 4.1 Principal road-link attributes

The associated study uses the following principal attributes:

| Attribute | Description |
|---|---|
| Origin node | Starting node of a directed link |
| Destination node | Ending node of a directed link |
| Road class | Functional road category |
| Length | Length of the road segment |
| Lane count | Number of lanes, where available or assigned |
| Direction | Directed connectivity of the link |
| Road name | Road name, where available |

The exact column names, data types, units, missing-value conventions, and coordinate reference system should be documented in the data dictionary accompanying the released files.

### 4.2 Road hierarchy

The research representation uses three functional road classes:

1. **Arterial roads:** Higher-level roads intended to serve major urban movement.
2. **Collector roads:** Roads connecting local streets and higher-level roads.
3. **Local roads:** Roads primarily serving local access and neighborhood movement.

These classes provide the road hierarchy used in the associated vehicle–road suitability model. They should not automatically be interpreted as official administrative road classifications.

## 5. Heterogeneous vehicle representation

The associated research evaluates routing for five vehicle classes:

1. Motorcycle
2. CNG (auto-rickshaw)
3. Private car
4. Bus
5. Rickshaw/bicycle

Vehicle-specific road-space lengths and road-class suitability coefficients are used in the research's routing and congestion formulations.

The suitability coefficient is defined as

\[
A_{e,v}\in\{0,0.5,1.0\},
\]

where \(e\) represents a road link and \(v\) represents a vehicle class.

- \(A_{e,v}=0\): The link is prohibited for that vehicle class under the adopted research model.
- \(A_{e,v}=0.5\): The link is permitted but penalized.
- \(A_{e,v}=1.0\): The link is fully suitable under the adopted model.

The experimental suitability matrix is:

| Vehicle class | Road-space length (m) | Arterial | Collector | Local |
|---|---:|---:|---:|---:|
| Motorcycle | 2.0 | 1.0 | 1.0 | 1.0 |
| CNG (auto-rickshaw) | 3.0 | 0.5 | 1.0 | 1.0 |
| Private car | 4.5 | 1.0 | 1.0 | 0.5 |
| Bus | 10.0 | 1.0 | 0.5 | 0.0 |
| Rickshaw/bicycle | 2.5 | 0.0 | 0.5 | 1.0 |

**Important:** These values are model assumptions used in the associated experiments. They should not be interpreted as a universally applicable legal classification of vehicle access in Dhaka. Researchers using the dataset for other purposes should validate or replace the suitability matrix according to their own application.

## 6. Associated synthetic routing workload

The associated research uses a controlled synthetic routing workload to evaluate different routing methods under common network conditions.

The workload contains **1,450 synthetic routing requests distributed across ten routing steps**.

The number of requests at step \(t\) is defined as

\[
|Q(t)|=100+10t,\qquad t=0,\ldots,9.
\]

Thus, the workload increases from 100 requests at the first step to 190 requests at the final step.

Each routing request includes the following conceptual information:

- Source node
- Destination node
- Vehicle class
- Vehicle count
- Priority value
- Routing step

The associated experiments also use synthetically generated background traffic to initialize the traffic state of directed links for each routing step and vehicle class.

The routing requests and initial background traffic are held consistent across the compared methods within the experimental protocol.

**Synthetic-data disclosure:** These requests and background traffic inputs are generated for controlled experiments. They are not a collection of observed trips, GPS trajectories, household travel surveys, or directly measured real-time traffic flows from Dhaka.

The synthetic workload should be included in this repository only if the corresponding request-generation procedure or files are actually released.

## 7. Potential research applications

Researchers may use the dataset, subject to the applicable source-data licenses and repository license, for:

- Urban road-network representation and graph analysis.
- Shortest-path and alternative-path routing.
- Heterogeneous vehicle routing.
- Road-class-aware path planning.
- Congestion-aware route optimization.
- Ant Colony Optimization and other metaheuristic methods.
- Routing algorithm benchmarking.
- Computational efficiency and scalability analysis.
- Synthetic traffic-demand experiments.
- Ablation studies of routing constraints and search strategies.
- Transportation-network visualization and exploratory analysis.

The dataset can also support comparisons among conventional shortest-path algorithms, sequential routing methods, and coordinated routing strategies, provided that the comparison protocol and traffic assumptions are clearly documented.

## 8. Repository structure

The repository is intended to organize the released network data and associated documentation in a transparent, reproducible format.

A recommended structure is:

```text
dhaka-urban-road-network-dataset/
├── README.md
├── LICENSE
├── CITATION.cff
├── data/
│   ├── nodes/
│   ├── edges/
│   └── network_attributes/
├── metadata/
│   ├── data_dictionary.csv
│   └── dataset_description.md
├── examples/
└── CHANGELOG.md
```

The folders above describe a recommended organization, not a guarantee that every file is currently present.

Before publication, document the actual filenames, formats, field definitions, coordinate reference system, and download or access instructions for every released data file.

If synthetic requests, background traffic, vehicle-suitability matrices, or benchmark results are released, document them in clearly named directories with corresponding schemas and descriptions.

## 9. Data formats and data dictionary

The exact data formats should be identified from the files included in the repository.

For example, if CSV files are provided, the node file may describe node identifiers and geographic coordinates, while the directed-link file may describe origin and destination node identifiers, road class, length, lane count, and road name.

These are suggested schemas, not assertions about the current file structure.

A complete data dictionary should describe:

| Field information | Required documentation |
|---|---|
| Field name | Exact column name |
| Data type | Integer, decimal, string, or other type |
| Definition | Meaning of the field |
| Units | For example, metres for length |
| Missing values | How unavailable information is represented |
| Provenance | Manual digitization, OSM, or derived attribute |
| Processing | Any transformation or default assignment |

Researchers should verify node identifiers, directed-link endpoints, duplicate records, missing values, invalid coordinates, and graph connectivity before using the data in new experiments.

## 10. Data quality and interpretation

The dataset combines manually digitized information with data derived from OpenStreetMap. As a result, data completeness and attribute provenance may vary between network components.

Users should consider the following points when interpreting or extending the dataset:

- The network reflects the source data and processing decisions used when it was constructed.
- Road names, lane counts, and classifications may be incomplete or inconsistent.
- Assigned default values should be distinguished from directly sourced attributes.
- Directed links should be checked for connectivity and directional consistency.
- The network should not be assumed to represent current road conditions without an updated verification process.
- Vehicle–road suitability coefficients are research-model assumptions.
- Synthetic routing requests and background traffic should be distinguished from observed traffic data.
- The network's size alone does not establish that it is complete or representative of all Dhaka roads.

No claim of comprehensive field validation, traffic-sensor validation, or temporal updating should be made unless supporting evidence is supplied.

## 11. Reproducibility

For reproducible research, users should record:

1. The dataset version and download date.
2. The network preprocessing steps.
3. The source and version of OSM-derived data, where available.
4. The coordinate reference system.
5. The treatment of missing road attributes.
6. The routing-request generation procedure.
7. The background traffic initialization method.
8. The vehicle–road suitability matrix.
9. The algorithm parameters and random seeds.
10. The evaluation metrics and baseline configurations.

When using this dataset with routing algorithms, comparisons should use consistent source–destination requests, vehicle classes, initial network conditions, and evaluation definitions wherever the experiment requires a controlled comparison.

## 12. Data sources and attribution

The network incorporates information from the following sources:

**Manually digitized road information:** A central-Dhaka road-network component prepared through QGIS-assisted mapping and visual inspection.

**OpenStreetMap:** An additional road-network component retrieved through the Overpass API.

OpenStreetMap data is made available under the Open Database License (ODbL). Users should consult the current OpenStreetMap attribution and licensing guidance and assess the obligations applicable to their use, modification, redistribution, and combination of OSM-derived data.

Official guidance: https://www.openstreetmap.org/copyright

Users should retain appropriate attribution and document the provenance of OSM-derived content in any redistribution or derivative dataset.

The presence of OSM-derived data does not mean that every manually digitized or independently created component automatically has the same licensing terms. The repository maintainer should verify the rights and applicable terms for each component before assigning a license to the complete dataset.

## 13. Limitations

This dataset has several important limitations.

**Geographic and temporal coverage:** The network represents the study area and source data used in its construction. It should not be treated as a continuously updated or exhaustive representation of Dhaka's road infrastructure.

**Attribute completeness:** Some attributes may be unavailable, estimated, manually assigned, or derived from source classifications.

**Traffic observations:** The associated routing experiments use synthetic requests and background traffic. The experimental workload does not, by itself, establish correspondence with observed traffic demand.

**Vehicle compatibility:** The suitability coefficients encode assumptions adopted for the research experiments. Their applicability may vary with local regulations, road geometry, vehicle dimensions, and operating conditions.

**Generalizability:** Findings obtained from this network and synthetic workload should be evaluated before being generalized to other cities, observed traffic environments, or real-time navigation systems.

**Operational use:** The dataset has been prepared for research. It should not be used as the sole basis for real-world navigation, emergency routing, road-access decisions, or transport-policy implementation without appropriate validation.

## 14. License and terms of use

The repository maintainer should specify a license that reflects the rights associated with the original and derived data.

Before adding a `LICENSE` file, verify:

- The rights associated with the manually digitized network.
- The attribution and redistribution obligations associated with OSM-derived data.
- The terms for any third-party datasets, maps, or supplementary materials.
- Whether the chosen license can legally cover all files being distributed.

Do not assume that a permissive software license automatically overrides the licensing terms of underlying geospatial data.

Until the applicable terms have been verified and a license has been added, users should not assume that all repository contents are freely reusable for every purpose.

## 15. Citation

If you use this dataset in academic research, please cite the dataset repository and the associated research publication when available.

**Dataset repository:**

Sourav Debnath. *Dhaka Urban Road Network Dataset*. GitHub repository:  
https://github.com/souravdebnath109/dhaka-urban-road-network-dataset

**Associated research manuscript:**

*Road Condition-Aware Heterogeneous Traffic Routing via Modified Ant Colony Optimization on a Large-Scale Urban Network.*

The publication's final bibliographic information and DOI should be added here after publication.

For long-term dataset citation, the maintainer is encouraged to archive a specific release in a service such as Zenodo and obtain a version-specific DOI. Cite the exact release used in the research rather than relying only on a link to the repository's latest version.

## 16. Updates and versioning

Dataset changes should be documented through versioned releases and a changelog.

When a new version is published, record:

- The release date and version number.
- Changes to node and directed-link counts.
- Changes in road attributes or classifications.
- Corrections to network connectivity or topology.
- Changes to data sources or preprocessing.
- Changes to synthetic workloads or metadata, if included.
- Compatibility with previously published experiments.

Avoid replacing a previously cited dataset version without preserving the original release or documenting the changes.

## 17. Contact and contributions

Researchers interested in using, evaluating, or extending this dataset are encouraged to consult the repository documentation and open a GitHub issue for questions about the released files.

**Repository:** [dhaka-urban-road-network-dataset](https://github.com/souravdebnath109/dhaka-urban-road-network-dataset)

### Contributors

- **[Sourav Debnath](https://github.com/souravdebnath109)** — Dataset development, research, and repository maintenance.
- **[Azrof88](https://github.com/Azrof88)** — Contribution to dataset development.

Contributions that improve documentation, data validation, reproducibility, or transparent reporting of limitations are welcome, subject to the applicable data-source terms.

## 18. Disclaimer

This dataset is provided for research and educational purposes. Users are responsible for verifying its suitability for their applications, respecting applicable licenses and attribution requirements, and validating the accuracy of any derived analyses or conclusions.

The dataset does not constitute an official government road inventory, a live traffic information service, or a guarantee of real-world road accessibility.

---

**Maintainer:** [Sourav Debnath](https://github.com/souravdebnath109)

**Contributors:** [Sourav Debnath](https://github.com/souravdebnath109), [Azrof88](https://github.com/Azrof88)

**Repository:** [dhaka-urban-road-network-dataset](https://github.com/souravdebnath109/dhaka-urban-road-network-dataset)

**Research domain:** Urban transportation, road-network analysis, heterogeneous traffic routing, and transportation optimization.
