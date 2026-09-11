# Advanced Player Performance & Market Analysis — FIFA 22

## Overview

This project performs exploratory data analysis (EDA) and market analysis on a **FIFA 22 player dataset**. The analysis connects player performance with market economics by examining:

- Overall and potential ratings
- Market value
- Weekly wages
- Age and age groups
- Player positions
- League-level comparisons
- Value-to-wage efficiency
- Relationships between performance, value, wages, age, and potential

The notebook is designed to produce both analytical outputs and **report-ready PNG visualizations**.

## Dataset

The notebook uses FIFA 22 player data and is configured to work with:

- CSV files
- Excel (`.xlsx` / `.xls`)
- ZIP files containing CSV or Excel data

The executed notebook used a FIFA 22 female-player workbook.

The loaded dataset contains:

- **391 players**
- **110 original columns**

The dataset includes player identity information, positions, ratings, market/economic attributes, and FIFA attribute fields.

> **Important:** The workbook used during execution did not contain usable `league_name` values. Therefore, the requested top-five-league filter could not be applied to the executed dataset, and the notebook automatically fell back to all 391 FIFA 22 players. The notebook can be configured with `STRICT_TOP5_FILTER=True` if strict filtering is required.

## Objectives

1. Identify the highest-rated players.
2. Examine the highest-valued players.
3. Compare player wages.
4. Study the relationship between age and overall rating.
5. Study the relationship between wages and overall rating.
6. Analyze market value against potential and age group.
7. Measure performance using a derived performance index.
8. Identify potentially undervalued players using performance and wage efficiency.
9. Identify young players with high future potential.
10. Compare player characteristics across leagues and positions when league data is available.
11. Explore correlations between important performance and market variables.

## Tech Stack

- **Python 3.12**
- **Pandas** — data loading, cleaning, transformation, and analysis
- **NumPy** — numerical operations and feature engineering
- **Matplotlib** — visualization and image export
- **Seaborn** — statistical visualization
- **Jupyter Notebook** — analysis environment

## Project Structure

```text
.
├── Advanced_Player_Performance_Market_Analysis_FIFA22_executed.ipynb
├── README.md
└── outputs/
    ├── head.png
    ├── shape.png
    ├── describe.png
    ├── top_15_overall.png
    ├── top_15_value.png
    ├── top_15_wage.png
    ├── age_vs_overall.png
    ├── wage_vs_overall.png
    ├── value_vs_potential_age_group.png
    ├── performance_index_distribution.png
    ├── value_to_wage_ratio_boxplot.png
    ├── league_comparison.png
    ├── wage_distribution_per_league.png
    ├── age_distribution_per_league.png
    ├── position_distribution.png
    ├── avg_performance_by_position.png
    ├── correlation_heatmap.png
    ├── top_5_undervalued_players.png
    └── top_5_future_stars.png
```

## Data Processing

### Column normalization

The notebook standardizes common FIFA column names, including:

| Original | Standardized |
|---|---|
| `Name` | `short_name` |
| `LongName` | `long_name` |
| `Overall` | `overall` |
| `Potential` | `potential` |
| `Value` / `Value (?)` | `value_eur` |
| `Wage` / `Wage (?)` | `wage_eur` |
| `Age` | `age` |
| `Club` / `Club Name` | `club_name` |
| `League` / `League Name` | `league_name` |
| `Position` / `Positions` | `player_positions` |

### Missing-value handling

- Numeric missing values are filled using the **median** of the corresponding column.
- Missing text values are replaced with `Unknown`.
- Market value and wage fields are converted into numeric values for analysis.

### Derived units

Two market variables are converted into easier-to-read units:

- `value_millions` = market value in millions of euros
- `wage_thousands` = wage in thousands of euros

The first listed player position is also extracted as `primary_position`.

## Feature Engineering

Three main analytical features are created.

### 1. Performance Index

```text
performance_index = (overall + potential) / 2
```

This provides a combined indicator of current rating and future potential.

### 2. Value-to-Wage Ratio

```text
value_to_wage_ratio = value_millions / wage_thousands
```

This is used as a simple market-efficiency indicator.

Higher values can highlight players whose market value is relatively high compared with their wage.

### 3. Age Group

Players are grouped into:

| Age | Group |
|---|---|
| ≤ 22 | Young |
| 23–30 | Prime |
| > 30 | Veteran |

## Analysis & Visualizations

### Player Rankings

The project generates rankings for:

- Top 15 players by overall rating
- Top 15 players by market value
- Top 15 players by weekly wage

These rankings provide a quick view of elite sporting assets and major financial commitments.

### Relationship Analysis

The following relationships are visualized:

- **Age vs Overall Rating**
- **Wage vs Overall Rating**
- **Market Value vs Potential by Age Group**

These plots help examine how player quality, age, potential, and financial valuation interact.

### Performance Distribution

A distribution plot is created for the `performance_index` to understand the overall quality distribution within the player pool.

### Market Efficiency

A boxplot of the `value_to_wage_ratio` is used to identify unusually high or low market-efficiency values.

The project also creates a **Top 5 Undervalued Players** table using players above the 70th percentile of performance and ranking them using:

```text
undervalued_score = performance_index / (wage_thousands + 1)
```

This is a project-specific analytical indicator, not an official football-market valuation model.

### Future Stars

The **Top 5 Future Stars** analysis focuses on players in the `Young` age group and ranks them by:

1. Potential
2. Overall rating

This supports a recruitment strategy focused on future development and potential resale value.

### League Analysis

When valid league labels are available, the notebook compares:

- Average performance index by league
- Wage distribution by league
- Age distribution by league

For the executed workbook, league labels were unavailable, so these comparisons are represented using the fallback group **`League Not Available`** rather than pretending that the data contains valid top-five league information.

### Position Analysis

The project examines:

- Player count by primary position
- Average performance index by primary position

This can help identify positions with strong performance levels or limited player supply.

### Correlation Analysis

A correlation heatmap examines relationships among variables such as:

- Age
- Overall
- Potential
- Market value
- Wage
- Performance index
- Value-to-wage ratio
- Pace
- Shooting
- Passing
- Dribbling
- Defending
- Physic

## Key Analytical Ideas

The notebook is designed around two practical recruitment perspectives:

### Immediate value optimization

High-performing players with relatively low wages may represent potentially efficient contracts.

### Future asset growth

Young players with high potential may represent development opportunities and future resale value.

The project therefore combines **sporting performance** with **market economics**, rather than relying on overall rating alone.

## Execution Notes

The notebook automatically creates an `outputs/` directory and saves report-ready images at high resolution.

The original execution produced the following dataset statistics:

```text
Rows: 391
Columns: 110

Mean Overall: 76.71
Mean Potential: 79.51
Mean Age: 27.00

Minimum Overall: 60
Maximum Overall: 92

Minimum Potential: 66
Maximum Potential: 93

Minimum Age: 18
Maximum Age: 38
```

The original dataset contains missing market-value and wage entries; the cleaning stage handles these before the engineered analysis is performed.

## Limitations

- The executed workbook has missing `league_name` information, so genuine league comparisons cannot be interpreted from the executed output.
- The `value_to_wage_ratio` and `undervalued_score` metrics are analytical heuristics created for this project; they should not be treated as professional transfer-market valuation models.
- FIFA ratings are game-derived attributes and should not be interpreted as real-world scouting measurements without additional football performance data.
- The analysis is primarily exploratory and descriptive; it does not establish causal relationships.

## How to Run

### 1. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn openpyxl jupyter
```

### 2. Open the notebook

```bash
jupyter notebook
```

Open:

```text
Advanced_Player_Performance_Market_Analysis_FIFA22_executed.ipynb
```

### 3. Configure the dataset path

Update the `DATA_PATH` variable in the notebook to point to your local FIFA 22 dataset.

The loader supports:

```text
.csv
.xlsx
.xls
.zip
```

### 4. Execute the notebook

Run the cells from top to bottom.

The generated charts and report tables will be saved inside:

```text
outputs/
```

## Conclusion

This project demonstrates how football player data can be transformed into actionable exploratory insights by combining **performance ratings, potential, age, market value, wages, positions, and market-efficiency indicators**.

The analysis supports two broad recruitment strategies:

- **Find efficient current performers**
- **Find high-potential young assets**

It also demonstrates a complete data-analysis workflow covering data loading, cleaning, feature engineering, visualization, ranking, segmentation, and interpretation.
