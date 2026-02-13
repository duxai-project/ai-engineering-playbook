# Seaborn – Quick Memory (Command + Output)

Note: Some outputs may vary (rendered plots, palettes). Some functions shown below are deprecated in newer Seaborn versions; see the "Deprecated vs New" section.

Seaborn is a statistical visualization library built on Matplotlib. It provides high-level plot functions and attractive defaults, especially for DataFrame-based workflows.

## Setup
Import Seaborn and helpers, then load sample datasets used throughout.

```python
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
```

Note: If you are using Jupyter/Colab, `%matplotlib inline` shows plots inside the notebook.

```python
tips = sns.load_dataset('tips')
tips.head()
```

```python
flights = sns.load_dataset('flights')
```

---

## Distribution Plots
Visualize numeric distributions with histograms, KDEs, and rug plots.

```python
sns.distplot(tips['total_bill'])
# Deprecated: use histplot/kdeplot or displot
```

```python
sns.distplot(tips['total_bill'], kde=False, bins=30)
```

```python
sns.kdeplot(tips['total_bill'])
sns.rugplot(tips['total_bill'])
```

```python
sns.rugplot(tips['tip'])
```

**Modern equivalents:**
- `sns.histplot(data=..., x=..., bins=...)`
- `sns.kdeplot(data=..., x=...)`
- `sns.displot(data=..., x=..., kind='hist'|'kde'|'ecdf')`

---

## Joint & Pair Plots
Explore relationships between variables with combined or multi-plot views.

```python
sns.jointplot(x='total_bill', y='tip', data=tips, kind='scatter')
```

```python
sns.jointplot(x='total_bill', y='tip', data=tips, kind='hex')
```

```python
sns.jointplot(x='total_bill', y='tip', data=tips, kind='reg')
```

```python
sns.pairplot(tips)
```

```python
sns.pairplot(tips, hue='sex', palette='coolwarm')
```

---

## Modern Figure-Level API
Use figure-level functions for fast, consistent faceting and small multiples.

```python
sns.relplot(data=tips, x='total_bill', y='tip', hue='sex', kind='scatter')
```

```python
sns.relplot(data=tips, x='total_bill', y='tip', col='time', row='smoker', kind='scatter')
```

```python
sns.catplot(data=tips, x='day', y='total_bill', hue='sex', kind='box')
```

```python
sns.displot(data=tips, x='total_bill', kind='hist')
```

```python
sns.displot(data=tips, x='total_bill', kind='kde')
```

```python
sns.displot(data=tips, x='total_bill', kind='ecdf')
```

---

## Categorical Plots
Compare categories with summary stats and distribution-focused plots.

```python
sns.barplot(x='sex', y='total_bill', data=tips)
```

```python
import numpy as np
sns.barplot(x='sex', y='total_bill', data=tips, estimator=np.std)
```

```python
sns.countplot(x='sex', data=tips)
```

```python
sns.boxplot(x='day', y='total_bill', data=tips, palette='rainbow')
```

```python
sns.boxplot(data=tips, palette='rainbow', orient='h')
```

```python
sns.boxplot(x='day', y='total_bill', hue='smoker', data=tips, palette='coolwarm')
```

```python
sns.violinplot(x='day', y='total_bill', data=tips, palette='rainbow')
```

```python
sns.violinplot(x='day', y='total_bill', data=tips, hue='sex', split=True, palette='Set1')
```

```python
sns.stripplot(x='day', y='total_bill', data=tips, jitter=True)
```

```python
sns.stripplot(x='day', y='total_bill', data=tips, jitter=True, hue='sex', palette='Set1', split=True)
```

```python
sns.swarmplot(x='day', y='total_bill', data=tips)
```

```python
sns.swarmplot(x='day', y='total_bill', hue='sex', data=tips, palette='Set1', split=True)
```

```python
sns.violinplot(x='tip', y='day', data=tips, palette='rainbow')
sns.swarmplot(x='tip', y='day', data=tips, color='black', size=3)
```

**Modern equivalent of factorplot:**
- `sns.catplot(x=..., y=..., data=..., kind='bar'|'box'|'violin'|...)`

---

## Matrix Plots
Show matrix-like data such as correlations or pivots with heatmaps.

```python
sns.heatmap(tips.corr())
```

```python
sns.heatmap(tips.corr(), cmap='coolwarm', annot=True)
```

```python
pvflights = flights.pivot_table(values='passengers', index='month', columns='year')
sns.heatmap(pvflights)
```

```python
sns.heatmap(pvflights, cmap='magma', linecolor='white', linewidths=1)
```

```python
sns.clustermap(pvflights)
```

```python
sns.clustermap(pvflights, cmap='coolwarm', standard_scale=1)
```

---

## Grids
Build multi-plot layouts and customize how each subplot is drawn.

```python
iris = sns.load_dataset('iris')
g = sns.PairGrid(iris)
```

```python
g = sns.PairGrid(iris)
g.map(plt.scatter)
```

```python
g = sns.PairGrid(iris)
g.map_diag(plt.hist)
g.map_upper(plt.scatter)
g.map_lower(sns.kdeplot)
```

```python
sns.pairplot(iris)
```

```python
sns.pairplot(iris, hue='species', palette='rainbow')
```

```python
g = sns.FacetGrid(tips, col='time', row='smoker')
```

```python
g = sns.FacetGrid(tips, col='time', row='smoker')
g.map(plt.hist, 'total_bill')
```

```python
g = sns.FacetGrid(tips, col='time', row='smoker', hue='sex')
g.map(plt.scatter, 'total_bill', 'tip').add_legend()
```

```python
g = sns.JointGrid(x='total_bill', y='tip', data=tips)
```

```python
g = sns.JointGrid(x='total_bill', y='tip', data=tips)
g.plot(sns.regplot, sns.distplot)
# distplot is deprecated
```

---

## Regression Plots
Fit and visualize linear relationships, with grouping and faceting.

```python
sns.lmplot(x='total_bill', y='tip', data=tips)
```

```python
sns.lmplot(x='total_bill', y='tip', data=tips, hue='sex')
```

```python
sns.lmplot(x='total_bill', y='tip', data=tips, hue='sex', palette='coolwarm')
```

```python
sns.lmplot(
...     x='total_bill', y='tip', data=tips, hue='sex', palette='coolwarm',
...     markers=['o', 'v'], scatter_kws={'s': 100}
... )
```

```python
sns.lmplot(x='total_bill', y='tip', data=tips, col='sex')
```

```python
sns.lmplot(x='total_bill', y='tip', data=tips, row='sex', col='time')
```

```python
sns.lmplot(x='total_bill', y='tip', data=tips, col='day', hue='sex', palette='coolwarm')
```

```python
sns.lmplot(
...     x='total_bill', y='tip', data=tips, col='day', hue='sex', palette='coolwarm',
...     aspect=0.6, size=8
... )
```

---

## Style and Color
Control global look-and-feel, palettes, contexts, and figure sizing.

```python
sns.countplot(x='sex', data=tips)
```

```python
sns.set_style('white')
sns.countplot(x='sex', data=tips)
```

```python
sns.set_style('ticks')
sns.countplot(x='sex', data=tips, palette='deep')
```

```python
sns.countplot(x='sex', data=tips)
sns.despine()
```

```python
sns.countplot(x='sex', data=tips)
sns.despine(left=True)
```

```python
plt.figure(figsize=(12, 3))
sns.countplot(x='sex', data=tips)
```

```python
sns.lmplot(x='total_bill', y='tip', data=tips, size=2, aspect=4)
```

```python
sns.set_context('poster', font_scale=4)
sns.countplot(x='sex', data=tips, palette='coolwarm')
```

---

## Objects Interface (Newer)
Compose complex plots with a more consistent, Matplotlib-like API.

```python
from seaborn import objects as so
(
...   so.Plot(tips, x='total_bill', y='tip', color='sex')
...   .add(so.Dot())
...   .add(so.Line(), so.PolyFit())
... )
```

---

## Deprecated vs New
Quick mapping of old function names to the current recommended ones.

Seaborn has renamed or replaced several older functions. If you use a newer version, prefer these:
- `distplot` -> `histplot`, `kdeplot`, or `displot`
- `factorplot` -> `catplot`
- `pairplot` remains, but `PairGrid` gives more control

---

## Key Ideas
The core mental model to keep Seaborn usage consistent and clean.

Core takeaways to remember when working with Seaborn.
- **Seaborn works best with DataFrames**: pass column names directly.
- **Figure-level vs axes-level**: functions like `jointplot`, `pairplot`, `catplot`, `lmplot` create their own figure.
- **Use `hue` for grouping** and `palette` for color control.
- **Grid objects** (`FacetGrid`, `PairGrid`, `JointGrid`) allow custom multi-plot layouts.
- **Style is global**: `set_style()` and `set_context()` affect subsequent plots.

---

## Best Practices
Practical tips for using Seaborn effectively in modern workflows.
- **Prefer figure-level for faceting**: `relplot`, `catplot`, `displot` scale better for small multiples.
- **Axes-level for fine control**: use `scatterplot`, `lineplot`, `histplot`, `boxplot`, etc. inside custom Matplotlib layouts.
- **Mind deprecations**: avoid `distplot` and `factorplot` in new code.
- **Set themes once**: call `sns.set_theme()` (or `set_style`/`set_context`) at the top of a notebook/script.
- **Control legends**: use `hue` for grouping, and consider `legend=False` when cluttered.


