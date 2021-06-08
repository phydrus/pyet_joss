---
title: '`pyet`: A Python package for estimating evaporation'
tags:
  - Python
  - Evaporation
  - Water balance
  - Hydrology
authors:
  - name: Matevž Vremec
    orcid: 0000-0002-2730-494X
    affiliation: 1 
  - name: Raoul Collenteur
    orcid: 0000-0001-7843-1538
    affiliation: 1
affiliations:
 - name: Institute of Earth Sciences, NAWI Graz Geocenter Austria, University of Graz, Austria
   index: 1
date: 2021
bibliography: paper.bib
---

# Summary

The evaporation of water from land surfaces to the atmosphere is a major component of the water cycle. Accurate estimates of the flux are essential to the water and agricultural sectors. As direct measurement of evaporation is difficult, the evaporation flux is commonly estimated from more easily obtained meteorological data using empirical formulas. There is a wide variety of such formulas, ranging from complex physically based methods (e.g., Penman-Monteith), to simpler temperatue-based methods (e.g., Oudin, Hamon). Most of the formulas tend to estimate the potential evaporation (PE), which is the maximum amount of water that can be evaporated if enough water were available. Water resources experts rely on these formulas to get a first impression of the hydrological water balance. Due to the diversity in the level of input data, process representation and assumptions, implementing these methods can be challenging and prone to errors. The goal of `pyet` is to provide a Python package that includes a wide range of methods for the estimation of PE, is fully documented and easy to use. Currently, `pyet` includes eighteen different methods to estimate daily PE and various methods to estimate surface and aerodynamic resistance and other missing meteorological data. 

# Statement of Need

There exists a number of standardized calculation methods of potential and reference evaporation, from which the benchmark reports of the American Society of Civil Engineers [@walter2000asce] and the Food and Agriculture Organization [@allen1998crop] are amongs the most widely adopted. There are several scattered open-source Python packages that follow one of the benchark equations (@pyet0_2019, @eto_2019, @refet_2020 and @evap_2020), however none of these packages provide an exhaustive list of alternative methods or lack in testing and documentation. Outside of Python, the R package `Evapotranspiration` by @Danlu2016r is the most closely related project to `pyet` and provides similar functionalities to the R community. Hence, the aim of `pyet` is to provide a simple to use Python package, that includes a wide variety of evaporation formulas, to the water resources experts in the Python community. Allowing multiple levels of input data, `pyet` can be of importance in regions with sparsely distributed measurement stations or climate change studies, where observations or predictions of standard meteorological data (e.g., wind, relative humidity) are often not available.

# Estimation of Evaporation 

Evaporation is the process where a substance is converted from its liquid into its vapor phase. In this paper, the term evaporation is used to refer to the total evaporation from a land surface, comprising of transpiration (evaporation of water from inside the leaves), evaporation from bare soils and interception (evaporation of intercepted precipitation)[@miralles2020]. 

The evaporation flux is determined by meteorological conditions, whereas the availability of water determines if actual evaporation occurs at its potential rate (PE). Observations of actual evaporation are limited to point measurements and require expensive instrumentation, which is often not available for most practical applications. A common method of obtaining actual evaporation rates is thus by using hydrological models, which compute actual evaporation by reducing PE due to water limitations. These models require accurate input of PE, which is often calibrated to match regional and plot characteristics [@OUDIN2005290].

A common formula to determine the potential evaporation is using the Penman-Monteith formulation [@allen1998crop, @walter2000asce], which requires measurements of air temperature, relative humidity, wind speed and radiation. These requirements limit the applicability of the Penman-Monteith. Alternative methods that require less input data are therefore applied in regions where all standard meteorological variables are not measured or climate scenarios, that only offer projections of radiation and temperature. The study of @OUDIN2005290 gives an introduction on the importance of alternative methods to estimate PE. They report that in some regions the alternative PE methods outperform the Penman-Monteith method.

# Functionality

`pyet` currently contains 18 different PE methods (see table below). The package also provides utility functions to estimate missing meteorological data (e.g. solar and net radiation). The methods are currently implemented for 1D data (time series data). Future work will expand functionality to 2D and 3D data (Numpy array, XArray, and NetCDF). The implemented methods are benchmarked against open source data to ensure proper functioning of the methods [@allen1998crop].

Table 1: PE and surface/aerodynamic resistance methods included in `pyet`. T, Temperature; U, Wind Speed; 
D, Radiation; RH, Relative Humidity; $h_{crop}$, crop height; LAI, Leaf area index; $[CO_2]$ - atmospheric
$CO_2$ concentration. Adapted from @OUDIN2005290.

| Method            | Data needed     | PyEt Method       | Reference                   |
|-------------------|-----------------|-------------------|-----------------------------|
| Penman            | RH, T, U, D     |`penman`           |@penman1948natural           |
| Penman-Monteith   | RH, T, U, D     |`pm`               |@monteith1965evaporation     |
|                   |                 |                   |@schymanski_2017             |
| FAO-56            | RH, T, U, D     |`pm_fao56`         |@allen1998crop               |
| Priestley-Taylor  | T, D            |`priestley_taylor` |@priestley1972assessment     |
| Kimberly-Penman   | RH, T, U, D     |`kimberly_penman`  |@wright1982new               |
| Thom-Oliver       | RH, T, U, D     |`thom_oliver`      |@thom1977penman              |
| Blaney–Criddle    | T, D            |`blaney_criddle`   |@blaney1952determining       |
| Hamon             | T               |`hamon`            |@hamon1963estimating         |
| Romanenko         | RH, T           |`romanenko`        |@xu2001evaluation            |
| Linacre           | T               |`linacre`          |@linacre1977simple           |
| Turc              | T, D            |`turc`             |@xu2001evaluation            |
| Jensen–Haise      | T, D            |`jensen_haise`     |@jensen1963estimating        |
| McGuinness–Bordne | T, D            |`mcguinness_bordne`|McGuinness & Bordne          |
|                   |                 |                   |(1972)                       |
| Hargreaves        | T               |`hargreaves`       |Hargreaves & Samani          |
|                   |                 |                   |(1982)                       |
| Doorenbos–Pruitt  | RH, T, U, D     |`fao_24`           |@jensen1990evapotranspiration|
|(FAO-24)           |                 |                   |                             |
| Abtew             | T, D            |`abtew`            |@abtew1996evapotranspiration |
| Makkink           | T, D            |`makkink`          |@makkink1957testing          |
| Oudin             | T               |`oudin`            |@OUDIN2005290                |
| Aerodynamic       | U,($h_{crop}$)  |`calc_res_aero`    |@allen1998crop               |
| resistance        |                 |                   |                             |
| Surface           | (LAI),($[CO_2]$)|`calc_res_surf`    |@allen1998crop               |
| resistance        |                 |                   |@Yang2018HydrologicIO        |

# Example application

This example shows how `pyet` can be used to compute potential evaporation using different evaporation methods. The potential evaporation is estimated for the city of De Bilt (Netherlands), using data provided by the The Royal Netherlands Meteorological Institute (KNMI). The potential evaporation is computed using four different methods (Penman, Priestley-Taylor, Makking) and plotted for comparison.

``` python
# Import needed packages
import pandas as pd
import pyet

# Import meteorological data 
data = pd.read_csv("data/etmgeg_260.txt", skiprows=46, delimiter=",", 
                   skipinitialspace=True, index_col="YYYYMMDD", 
				   parse_dates=True)
meteo = pd.DataFrame({"tmean":data.TG/10, "tmax":data.TX/10, 
                      "tmin":data.TN/10, "wind":data.FG/10, 
					  "rh":data.UG, "rs":data.Q/100})				   
tmean, tmax, tmin, rh, wind, rs = [meteo[col] for col in meteo.columns]

# Compute Evaporation
et_penman = pyet.pm(tmean, wind, rs=rs, elevation=279, lat=0.813, 
					tmax=tmax, tmin=tmin, rh=rh)
et_pt = pyet.priestley_taylor(tmean, wind, rs=rs, elevation=279, 
							  lat=0.813, tmax=tmax, tmin=tmin, rh=rh)
et_makkink = pyet.makkink(tmean, rs, elevation=279)
```
In the code above `rs` is the incoming solar radiation [MJ m-2 d-1], `elevation` the altitude above sea level [m], `lat` the site latitude [rad], `tmean` the mean temperature, `tmax` and `tmin` the maximum and minimum temperature [°C] and `rh` the mean relative humidity [%]. The results show that PE differs based on the estimation method that is used, thus demonstrating the model uncertainty associated with the estimation of PE.

![Daily and cumulative potential evaporation for De Bilt estimated according to @monteith1965evaporation, 
@priestley1972assessment, @makkink1957testing and @OUDIN2005290.](Figure1.png)


# Concluding remarks

This paper presents `pyet`, a Python package to estimate daily potential evaporation from available meteorological data. Using `pyet`, users can estimate PE using 18 different methods with only a few lines of Python-code. At this stage (pyet v1.0), the methods are implemented for 1D data (e.g. time series data).
Further developments will focus on enabling 2D and 3D data input (Numpy Arrays, Array, and NetCDF files). 
`pyet` enables a simple PE estimation and comparison between different PE estimates, but also further usage of the methods in hydrological models and sensitivity analyses.
The `pyet` design allows simple extension of the software with new capabilities and new model options.
The authors welcome code contributions, bug reports, and feedback from the community to further improve the package. `pyet` is free and open-source software under the MIT license and is available at http://www.github.com/phydrus/pyet. Full documentation is available on ReadTheDocs (https://pyet.readthedocs.io). 
The notebook and data necessary to reproduce the figures in this manuscript are available through `pyet` GitHub page.

# Acknowledgements

The first author is partly funded by the Earth System Sciences programme of the Austrian Academy of 
Sciences (project ClimGrassHydro). The second author is funded by the Austrian Science Fund (FWF) under Research 
Grant W1256 (Doctoral Programme Climate 525 Change: Uncertainties, Thresholds and Coping Strategies).

# References