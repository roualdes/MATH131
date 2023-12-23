---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Week 04: Filling in some details

```{code-cell}
import numpy as np
import pandas as pd
import plotnine as pn

from plotnine.data import msleep
```

## DataFrame details






## Graphing details

```{code-cell}
df = msleep.dropna(subset = "vore")

odf = (df
    .groupby("vore", as_index = False)
    .aggregate(
        mean = ("sleep_total", np.mean),
        sd = ("sleep_total", np.std),
        count = ("sleep_total", np.size)))

odf["se"] = odf["sd"] / np.sqrt(odf["count"])
odf["ub"] = odf["mean"] + 1.96 * odf["se"]
odf["lb"] = odf["mean"] - 1.96 * odf["se"]

p = (pn.ggplot(odf)
    + pn.geom_point(pn.aes(x = odf["vore"].cat.codes, y = "mean"), color = "blue")
    + pn.geom_errorbar(pn.aes(x = odf["vore"].cat.codes, ymin = "lb", ymax = "ub"), color = "blue")
    + pn.geom_jitter(df, pn.aes(x = df["vore"].cat.codes, y = "sleep_total"), width = 0.25)
    + pn.scale_x_continuous(breaks = df["vore"].cat.codes.to_list(), labels = df["vore"].to_list())
    + pn.labs(y = "mean sleep total"))
p.draw()
```
