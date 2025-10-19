# ISPD 2025 Global Routing Contest: Evaluation Framework

This document outlines the evaluation metrics and benchmark specifications used in the ISPD 2025 Global Routing Contest. All criteria and data are sourced from the official contest framework [https://github.com/liangrj2014/ISPD25_contest/blob/main/index.md].

## 1. Evaluation Metrics

The overall quality of a global routing solution is quantified by a composite `final score`. This score is calculated as a weighted sum of key metrics—timing performance, power consumption, and routing congestion—benchmarked against median reference values derived from all contest submissions.

### 1.1. Overall Score Calculation

The total score is computed using the following formula:

$$
\text{final score} = w_1 \cdot (\text{WNS} - \text{WNS}_{\text{ref}}) + w_2 \cdot \frac{(\text{TNS} - \text{TNS}_{\text{ref}})}{N_{\text{endpoint}}} + w_3 \cdot (\text{TotalPower} - \text{TotalPower}_{\text{ref}}) + w_4 \cdot \text{OverflowScore}
$$

where:
- **`WNS`** (Worst Negative Slack): Represents the most critical timing violation in the design.
- **`TNS`** (Total Negative Slack): The sum of all negative slacks across all timing endpoints.
- **`TotalPower`**: The total power consumption of the design.
- **`OverflowScore`**: A penalty term for routing congestion, detailed below.
- **`WNS_ref`, `TNS_ref`, `TotalPower_ref`**: Median reference values for WNS, TNS, and Total Power, respectively, established from all contest submissions.
- **`N_endpoint`**: The total number of timing path endpoints in the specific benchmark case.
- **`w1, w2, w3, w4`**: Case-specific weight factors that tune the relative importance of each metric.

### 1.2. Overflow Score

The `OverflowScore` quantifies routing congestion across all GCell edges. It is calculated as the sum of penalties for all edges where routing demand exceeds capacity, using a layer-weighted exponential function.

$$
\text{OverflowScore} = \sum_{\text{edge} \in E} \text{OFWeight}[l] \cdot e^{s \cdot (d-c)}
$$

where:
- **`d`**: The routing demand on a given GCell edge.
- **`c`**: The available routing capacity of that edge.
- **`OFWeight[l]`**: A predefined weight factor for the metal layer `l` to which the edge belongs.
- **`s`**: A scaling factor whose value depends on the edge capacity `c`:

$$
s = \begin{cases}
0.5, & \text{if } c > 0 \\
1.5, & \text{if } c \leq 0 \quad \text{(e.g., routing blockages)}
\end{cases}
$$

---

## 2. Benchmark Suite

The contest utilizes a suite of 12 benchmarks, each with distinct characteristics and evaluation parameters.

### 2.1. Benchmark Characteristics

The fundamental statistics for each benchmark are detailed in the table below.

| ID | Benchmark Name | Clock Peroid $(\mu s)$ | #Endpoints | #Nets | GCell Dimensions |
|----|----------------------------|--------|------------|--------|-----------------------|
| 0 | `ariane` | 1.3 | 20.2K | 124K | 10 × 761 × 761 |
| 1 | `bsg` | 1.4 | 214K | 737K | 10 × 1384 × 1384 |
| 2 | `NVDLA` | 0.6 | 45.9K | 199K | 10 × 1120 × 1120 |
| 3 | `mempool_tile` | 6 | 13.4K | 136K | 10 × 428 × 428 |
| 4 | `mempool_group` | 3 | 348K | 3.27M | 10 × 1611 × 1610 |
| 5 | `mempool_cluster` | 3 | 1.08M | 12.0M | 10 × 3175 × 3175 |
| 6 | `ariane_hidden` | 8 | 20.2K | 106K | 10 × 646 × 646 |
| 7 | `bsg_hidden` | 52 | 215K | 768K | 10 × 1384 × 1384 |
| 8 | `NVDLA_hidden` | 70 | 45.9K | 158K | 10 × 1120 × 1120 |
| 9 | `mempool_tile_hidden` | 6 | 13.4K | 136K | 10 × 386 × 386 |
| 10 | `mempool_group_hidden` | 3 | 348K | 3.22M | 10 × 1611 × 1610 |
| 11 | `mempool_cluster_hidden` | 3 | 1.08M | 12.1M | 10 × 3719 × 3719 |

### 2.2. Scoring Parameters

The case-specific weights and reference metrics required for the score calculation are provided below. Note that the $w_4$ values are presented scaled by $10^{-8}$.

| ID | $w_1$ | $w_2$ | $w_3$ | $w_4 \times 10^{8}$ | $WNS_ref (ns)$ | $TNS_ref (ns)$ | $TotalPower_{ref} (W)$ |
|----|----------|---------|---------|---------------------|----------------|----------------|----------------------|
| 0 | -10 | -100 | 300 | 30 | -0.485 | -1398.39 | 0.646 |
| 1 | -10 | -100 | 25 | 4 | -0.440 | -10802.7 | 3.05 |
| 2 | -0.05 | -0.5 | 25 | 15 | -94.78 | -669471 | 2.96 |
| 3 | -1 | -10 | 300 | 70 | -0.695 | -3590.83 | 0.146 |
| 4 | -1 | -10 | 20 | 3 | -0.815 | -41740.2 | 7.82 |
| 5 | -1 | -10 | 0.3 | 0.5 | -0.680 | -79748.0 | 23.7 |
| 6 | -0.2 | -2 | 100 | 40 | -1.628 | -523.038 | 0.156 |
| 7 | -0.1 | -1 | 50 | 2 | 0.000 | 0.000 | 0.305 |
| 8 | -0.01 | -0.1 | 100 | 10 | 0.000 | 0.000 | 0.136 |
| 9 | -3 | -30 | 100 | 100 | -0.483 | -1920.72 | 0.145 |
| 10 | -0.5 | -5 | 3 | 4 | -0.680 | -40487.9 | 8.547 |
| 11 | -0.4 | -4 | 2 | 1 | -0.290 | -53249.0 | 24.26 |
