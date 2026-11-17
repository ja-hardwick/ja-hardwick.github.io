---
title: "topmodelR: implementation of TOPMODEL in R"
date: 2026-11-17
---
---
title: "Introducing topmodelR"
date: 2026-11-17
author: James Hardwick
---

# Introducing topmodelR

I've started building **topmodelR**, an R implementation of TOPMODEL, the classic rainfall-runoff model originally written in Fortran by [Beven (1995)](https://hess.copernicus.org/articles/25/527/2021/).

### What is TOPMODEL?

TOPMODEL (Beven and Kirkby, 1979) is a rainfall-runoff model built on the idea that topography controls where a catchment gets wet. It uses the topographic index, `ln(a / tan β)`, to predict which areas saturate first and generate runoff, using only a handful of parameters.


### Why build another one?

Others have already ported the Fortran code to R (see [here](https://github.com/ICHydro/topmodel/tree/master/topmodel)), but I'm writing my own for two reasons:

- **To learn** how R packages are built and how they develop over time.
- **To use it** in my own work with TOPMODEL.

### Installation

```r
# install.packages("remotes")
remotes::install_github("ja-hardwick/TopmodelR")
```

### Example:

```r
library(topmodelR)

ti     <- compute_ti(dem)          # dem: a terra SpatRaster
topidx <- make_classes(ti, n = 16)

out <- run_topmodel(rain = rain, ETp = ETp, topidx = topidx,
                    qs0 = 0.0001, Sr0 = 0.01, SrMax = 0.05,
                    m = 0.02, lnTe = 2, td = 20, dt = 1)
```

See `?topmodelR` for a glossary of every variable and parameter.

### What's next?

- **Semi-spatially distributed modelling**, so the model can represent how processes vary across the catchment rather than relying on the index distribution alone.
- **Land-use-specific parameters**, letting different land covers (e.g. forest, grassland, urban) have their own parameter values.
- **Green-Ampt infiltration**, to represent infiltration-excess runoff alongside TOPMODEL's saturation-excess runoff.

It's early days (v0.0.1). Feedback, bug reports and ideas are welcome on [GitHub](https://github.com/ja-hardwick/TopmodelR).