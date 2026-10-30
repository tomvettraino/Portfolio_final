# UK Bike-Share Market Feasibility & Multi-Criteria Selection Engine

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![pandas](https://img.shields.io/badge/pandas-Data%20Analysis-150458?logo=pandas)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c)](https://matplotlib.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A data-driven decision-making engine built in Python to evaluate the commercial viability and expansion potential for a sustainable bike-share operator across 16 major UK metropolitan areas.

The primary pipeline leverages public datasets—spanning census data, transport usage, infrastructure density, economic metrics, and local weather patterns—to build a **weighted Multi-Criteria Decision Analysis (MCDA)** framework.

---

## Key Highlights & Results

- **Data Aggregation & Schema Alignment**: Ingested and integrated 6 distinct datasets across demographic, infrastructural, economic, and meteorological dimensions.
- **Custom Spatial & Administrative Aggregations**: Harmonized disparate local authority boundaries (e.g., aggregating 33 London Boroughs and City of London records to uniform city-level representations).
- **Infrastructural & Behavioral Ratios**:
  - **Cycling Infrastructure Density**: Calculated the ratio of dedicated cycle paths to main road networks.
  - **No-Car Household Pct**: Extracted and synthesized Census car ownership categories to measure reliance on alternative transport.
- **Weighted Market Attractiveness Scoring**: Scaled all factors to a 0–100 range and applied strategic weights to derive a single metric: `MarketScore`.

### Top Ranked Cities by Market Viability

| Rank | City | MarketScore | Key Driving Factor(s) |
| :---: | :--- | :---: | :--- |
| **1** | **London** | **50.72** | High population, high zero-car household percentage (42.07%), strong GDHI |
| **2** | **Nottingham** | **45.05** | Leading cycle infrastructure density (0.626 infrastructure-to-main road ratio) |
| **3** | **Edinburgh** | **43.77** | High cycle infrastructure density (0.477) and lower car ownership |
| **4** | **Brighton** | **43.52** | High proportion of no-car households (37.45%) and strong local cycling demand |
| **5** | **Newcastle** | **43.06** | Balanced infrastructure and high proportion of commuters seeking alternatives |

---

## Methodology & Scoring Engine

The core market feasibility pipeline computes `MarketScore` as a weighted linear combination of normalized indicators:

$$\text{MarketScore} = \sum_{i=1}^{n} w_i \times S_{i, \text{normalized}}$$

Where each factor $S_{i}$ is min-max scaled or ratio-scaled to $[0, 100]$. Inverse factors (such as annual rainfall, existing competitors, and existing cycling commute saturation) are inverted prior to weighting so that higher normalized values always represent greater strategic opportunity.

```
       [ Raw Public Datasets ]
     (Population, Income, Rain,
    Cars, Commutes, Infra, etc.)
                 │
                 ▼
    [ Data Cleaning & Schema Matching ]
  • London Borough Consolidation
  • Car Availability Code Aggregation
  • Column Formatting & Deduplication
                 │
                 ▼
     [ Feature Engineering & Ratios ]
  • Infra-to-Main-Road Ratio
  • No-Car Household Percentage
  • Relative Rainfall & Market Saturation
                 │
                 ▼
       [ Min-Max Scaling (0–100) ]
  • Inversion for Friction Metrics
                 │
                 ▼
    [ Weighted Multi-Criteria Engine ]
                 │
                 ▼
      [ City Feasibility Ranking ]
```

### Strategic Weighting Matrix

| Metric / Indicator | Factor Name | Scaling Direction | Weight ($w_i$) | Rationale |
| :--- | :--- | :---: | :---: | :--- |
| **Car Availability** | `Car score` | Positive | **17.6%** | Primary driver; high no-car household % indicates higher baseline demand for shared transit. |
| **Infrastructure Density** | `Infrastructure` | Positive | **17.6%** | Dedicated cycling networks lower safety friction and encourage casual ridership. |
| **Unsaturated Commute Demand**| `Commute score` | Inverted | **17.6%** | Higher scores reflect room for adoption growth versus saturated modal split. |
| **Market Saturation** | `Competitors` | Inverted | **17.6%** | Lower competitive intensity improves market entry and operator unit economics. |
| **Population Base** | `Population` | Positive | **11.8%** | Total potential addressable market size. |
| **Economic Power** | `Gdhi score` | Positive | **11.8%** | Gross Disposable Household Income indicates discretionary spend capacity. |
| **Meteorological Friction** | `Weather score` | Inverted | **0.59%** | Rainfall impacts seasonal ride frequency; lower rainfall increases operational yield. |

---

## Data Pipeline Architecture

```
.
├── Data_MF/
│   ├── Population.csv
│   ├── Car ownership per city.csv
│   ├── Cycle paths.csv
│   ├── Commute times.csv
│   ├── avghouseholdincome.csv
│   ├── rain and rental per city.csv
│   └── Sustainable Mindset.csv
├── market_feasibility.ipynb      # Primary Market Analysis & Engine Pipeline
├── recycling_filtered.csv        # Output of local recycling rate filtering
├── recycling_cities.csv          # Local authority-to-city mapped dataset
├── README.md
└── requirements.txt
```

### Data Sources & Transformations

1. **Local Administrative Mapping**: Maps primary UK urban centers to broader Combined Authority/Region definitions for accurate Gross Disposable Household Income (GDHI) alignment.
2. **Infrastructure Harmonization**: Sums total cycle infrastructure kilometers against main road network length for all boroughs and councils to calculate network coverage density.
3. **Census Household Mobility**: Filters Census car availability statistics (`Code 0` = No car) and calculates local authority percentages.
4. **Sustainability Extension**: Integrates local authority recycling rates and sustainable mindset metrics to form a dual-index perspective (`SustainabilityScore` vs. `MarketScore`).

---

## Quickstart & Installation

### Prerequisites

- Python 3.9+
- Jupyter Notebook / JupyterLab

### Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/uk-bikeshare-market-feasibility.git
   cd uk-bikeshare-market-feasibility
   ```

2. **Install required packages:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch the analysis notebook:**
   ```bash
   jupyter notebook market_feasibility.ipynb
   ```

---

## Key Insights & Portfolio Context

This project demonstrates core skills in data manipulation, business analytics, and spatial data consolidation:

1. **Complex Entity Resolution**: Successfully mapped mismatched geographic levels across regional Census data, London borough boundaries, and Metropolitan Combined Authorities without dropping key records.
2. **Business-Aligned Analytics**: Translated disparate raw transport metrics into a configurable decision engine tailored for corporate market-entry strategy.
3. **Modular Metric Engine**: The vector-based weight matrix can be dynamically re-configured to simulate alternative entry strategies (e.g., prioritizing rapid market capture vs. operational margin optimization).

---

## License

Distributed under the MIT License. See `LICENSE` for more information.