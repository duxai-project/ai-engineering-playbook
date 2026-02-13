# Pandas Plotting – Quick Memory (Command + Output)

Note: Pandas plotting uses Matplotlib under the hood.

Use `DataFrame.plot(...)` and `Series.plot(...)` for fast exploratory charts directly from your data.

```python
import pandas as pd
import matplotlib.pyplot as plt
```

```python
# Example DataFrame (replace with your own df3)
df3 = pd.DataFrame({
...     'a': [1, 2, 3, 4, 5, 6],
...     'b': [6, 5, 4, 3, 2, 1],
...     'd': [10, 12, 9, 14, 13, 11]
... })
```

---

## Scatter Plot

Quick relationship view between two numeric columns.

```python
df3.plot.scatter(x='a', y='b', c='red', s=50, figsize=(12, 3))
plt.show()
```

---

## Histogram

Distribution of one numeric column.

```python
df3['a'].plot.hist()
plt.show()
```

```python
plt.style.use('ggplot')
df3['a'].plot.hist(alpha=0.5, bins=25)
plt.show()
```

---

## Box Plot

Compare distribution across multiple columns.

```python
df3[['a', 'b']].plot.box()
plt.show()
```

---

## KDE / Density

Smooth estimate of a variable distribution.

```python
df3['d'].plot.kde()
plt.show()
```

```python
df3['d'].plot.density(lw=5, ls='--')
plt.show()
```

---

## Area Plot

Stacked area-style trend chart over index.

```python
# `ix` is deprecated; use `.iloc` or `.loc`
df3.iloc[0:30].plot.area(alpha=0.4)
plt.show()
```

```python
f = plt.figure()
df3.iloc[0:30].plot.area(alpha=0.4, ax=f.gca())
plt.legend(loc='center left', bbox_to_anchor=(1.0, 0.5))
plt.show()
```

---

## Key Ideas

- Pandas plots are fast for EDA; use Seaborn/Matplotlib for deeper customization.
- `plt.style.use(...)` changes global style for following plots.
- Prefer `.loc`/`.iloc`; avoid `.ix` (removed in modern pandas).

