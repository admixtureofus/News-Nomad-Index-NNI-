# News Nomad Index (NNI)
### Measuring News Consumption Patterns Through Composite Index Design and Clustering

## Overview

The **News Nomad Index (NNI)** is a composite indicator designed to characterize how people consume news across channels with different levels of proximity to original news sources.

Using the 2025 Media Audience Survey, this project combines **information entropy and tier-weighted channel usage** to quantify news consumption patterns, examines the index's relationships with external variables, and identifies distinct audience segments through K-means clustering.

The project focuses on three questions:

- How can news consumption patterns be summarized in a meaningful composite index?
- Do the index components capture distinguishable dimensions of news consumption?
- Can audience segmentation reveal interpretable differences in news channel usage?

## Key Results

| Metric | Result |
|---|---:|
| Survey respondents | 6,000 |
| Respondents with news-channel usage during the reference week | 5,702 |
| Respondents with no recorded news-channel usage during the reference week | 298 (5.0%) |
| Variance explained by the first principal component | 82.9% |
| Correlation between the two NNI components | \(r = 0.658\) |
| Best silhouette score in the evaluated clustering configurations | 0.5535 |
| Selected number of clusters | 5 |

*The clustering results indicate moderate separation between groups, rather than perfectly distinct audience segments.*

## Methodology

### 1. Data Preparation and Channel Classification

The analysis uses the **2025 Media Audience Survey**, which contains 6,000 respondents and 223 questionnaire items.

Eleven news-consumption channels were grouped into three tiers according to their proximity to original news sources.

| Tier | Channel types |
|---|---|
| Tier 0 | Newspapers, television, radio, and magazines |
| Tier 1 | Online portals and podcasts |
| Tier 2 | Messengers, social media, video platforms, short-form content, and generative AI |

Weekly channel usage was constructed from news- and current-affairs-specific questionnaire items, combining weekday and weekend usage where applicable.

Missing values were checked against channel screening questions before being interpreted as zero usage, accounting for questionnaire skip logic. Respondents reporting no news-channel usage during the reference week were retained as a separate group rather than silently excluded from the analysis.

### 2. Composite Index Construction

The NNI combines two components representing complementary aspects of news consumption.

**Normalized Shannon Entropy (\(H_{\mathrm{norm}}\))**

Measures the diversity of news consumption across channels. A higher value indicates that usage is distributed more broadly across channels rather than concentrated in a small number of them.

**Tier-Weighted Deviation (\(D_{\mathrm{weighted}}\))**

Summarizes channel usage while accounting for the relative tiers assigned to the news channels.

The two components showed a correlation of \(r = 0.658\). Principal component analysis (PCA) indicated that the first principal component explained 82.9% of their variance. The final implementation used equal weights (\(\alpha = \beta = 0.5\)) to combine the components.

The resulting index is intended to summarize both channel diversity and tier-related consumption patterns in a single measure.

### 3. Construct Validity Checks

To examine whether the index behaved plausibly, the analysis explored its relationships with external variables not used directly in the index calculation, including:

- Trust in news
- Awareness of media outlets
- Willingness to pay for news
- Age

The results were mixed: not all hypothesized relationships were statistically supported, and the direction of the relationship with media-outlet awareness raised questions about the current tier boundaries.

These findings were treated as opportunities to reassess the measurement design rather than as evidence that every validity hypothesis had been confirmed.

### 4. Feature Engineering and K-Means Clustering

For audience segmentation, the eleven channel-level variables were consolidated into three tier-level usage-frequency features. This reduced the dimensionality of the feature space and helped limit redundancy among correlated channel variables.

Because the frequency variables were highly skewed, particularly for Tier 2, the analysis compared clustering configurations using different preprocessing strategies.

The selected approach applied:

1. `log1p` transformation to tier-level usage frequencies
2. Feature standardization
3. K-means clustering
4. Evaluation using silhouette score, Calinski–Harabasz index, and Davies–Bouldin index

The best evaluated configuration used **five clusters**, achieving a silhouette score of **0.5535**, compared with approximately 0.42–0.43 in the initial configurations.

Rare channels were retained because excluding them produced only a small improvement in silhouette score, from 0.5535 to 0.5598. The analysis also reported an Adjusted Rand Index (ARI) of 1.0 across multiple random seeds.

### 5. Reusable Scoring Concept

The notebooks explore a potential scoring workflow in which a respondent's weekly channel usage can be transformed into an NNI score and used to assign a cluster.

A reusable scoring function (`score_new_user()`) is considered as a possible extension of the analysis. This is a proposed analytical capability, not a deployed production service.

## Repository Structure

```text
.
├── 01_전처리.ipynb
├── 02_NNI산출.ipynb
├── 03_군집분석.ipynb
├── 뉴스유목민지수_NNI_최종정리.ipynb
├── 변수정의_tier매핑표.xlsx
└── README.md
```

| File | Description |
|---|---|
| `01_전처리.ipynb` | Prepares survey variables and constructs channel-level usage data |
| `02_NNI산출.ipynb` | Calculates the NNI components and examines their relationships with external variables |
| `03_군집분석.ipynb` | Compares clustering configurations and performs audience segmentation |
| `뉴스유목민지수_NNI_최종정리.ipynb` | Consolidates the index design and analysis results |
| `변수정의_tier매핑표.xlsx` | Documents variable definitions and channel-to-tier mappings |

## Tools and Techniques

- **Language:** Python
- **Data analysis:** pandas, NumPy
- **Statistical methods:** Shannon entropy, PCA, correlation analysis, construct validity checks
- **Machine learning:** K-means clustering, feature transformation, standardization, silhouette analysis, ARI
- **Data source:** 2025 Media Audience Survey

## Limitations

- **Construct validity is not fully established.** Some expected relationships were not statistically supported, and the outlet-awareness results suggest that the tier classification may need further review.
- **Cluster separation is moderate.** A silhouette score of 0.5535 supports useful segmentation, but does not imply that all five groups are sharply distinct.
- **Results depend on analytical choices.** Channel classification, index weights, feature transformations, and clustering parameters can affect the results.
- **External validation remains necessary.** Further testing on independent data would help establish the robustness and practical usefulness of the index.
- **Operational deployment has not been demonstrated.** The reusable scoring workflow is a potential extension rather than a production implementation.

## Project Takeaways

This project provided experience in translating a broad research question into a measurable indicator and an audience-segmentation workflow.

Key lessons included:

- Designing composite indicators requires more than combining variables; the meaning and validity of each component must also be examined.
- Questionnaire skip logic and zero-usage cases need explicit handling to avoid introducing measurement errors.
- Feature transformations can substantially affect distance-based clustering when input variables are highly skewed.
- A strong internal clustering metric does not replace substantive interpretation or external validation.

## Awards & Recognition

- **[Exellence Award]** — [Media Statistics Analysis & Utilization Competition]
  - Team project with 4 members
  - [News Article](https://url.kr/bfhnmq)


---

*This repository documents an analytical project using survey data. Any redistribution of the underlying survey microdata should follow the applicable data-use terms and privacy requirements.*
