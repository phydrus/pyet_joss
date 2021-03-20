---
title: 'PyEt: A Python package for the estimation of reference and potential evaporation'
tags:
  - Python
  - Evaporation
  - Water balance
  - Hydrology
  - Evapotranspiration
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

The evaporation (ET) of water from land surfaces to the atmosphere is a major component of the water cycle and 
accurate estimates of the flux are essential to the water and agricultural sector. As direct measurement of ET 
is difficult, the evaporation flux is commonly estimated from standard meteorological data using empirical 
formulas. There is a wide variety of such formulas, ranging from complex physically based methods
(e.g. Penamn-Monteith),to less weather-data demanding methods (e.g. Oudin, Hamon, ...). Most of the formulas 
tend to estimate the potential evaporation (PET), which is the maximum amount of water that will be evaporated 
if enough water were available. Water resources experts many times rely on these formulas to get a first 
impression of the field water balance, preferably using a simple and fast procedure with multiple options of 
data input. Consistently implementing these methods is difficult due to the significant diversity in the level 
of input data, process representation, assumptions and data requirements. Therefore, the goal of `PyEt` is to 
provide a Python package that includes a wide range of methods for the estimation of PET, is fully documented 
and easy to use. The structure of the package allows easy coupling to hydrological models and usage in complex 
sensitivity analysis. Currently, `PyEt` includes eighteen different methods to estimate daily PET and various 
methods to estimate surface and aerodynamic resistance and other missing meteorological data(see Table below). 

# Statement of Need

In the Python community, there already exist many tools to compute potential evaporation. For example, `PyETo` 
package includes the FAO-56 Penamn-Monteith, the Hargreaves and the Thornthwaite method 
(https://github.com/woodcrafty/PyETo), the `evaporation` package the FAO-56 Penamn-Monteith method 
(https://github.com/openmeteo/evaporation) and `RefET` the ASCE-EWRI Penamn-Monteith method 
(https://github.com/WSWUP/RefET). All these packages, however, lack in consideration of alternative PET 
formulations. A popular package that also includes alternative PET formulations is already developed in the R 
community by @Danlu2016r.

Thus, motivation behind `PyEt` is to provide a simple to use package, that incorporates multiple evaporation 
formulations, to the Python community. Allowing multiple levels of input data, `PyEt` can be of great importance
in regions with sparsely distributed networks of measurement stations or climate change studies, where 
observations or predictions of standard meteorological data (e.g. wind, relative humidity) often not available.

# Estimation of Evaporation 

Evaporation(ET) is the process where a substance is converted from its liquid into its vapor phase. In this paper,
the term evaporation is used to refer to the total evaporation from a land surface, comprising of transpiration 
(evaporation of water from inside the leaves), evaporation from bare soils and interception (evaporation of 
intercepted precipitation)[@miralles2020]. Representing one of the largest water fluxes from cathchments, the 
magnitude of the flux is crucial for water related application and studies at both regional and plot scale. 

Evaporation flux is influenced from meteorological conditions, but also the availability of water, which 
determines if actual evaporation will occur at its potential rate (PET). Observations of actual evaporation are 
limited to point measurements and require expensive instrumentation, which is often not available for most 
practical applications. A common method of obtaining actual evaporation rates is thus by using hydrological models, 
which compute actual evaporation by reducing the PET due to water limitations. These models therefore relly on 
accurate input of PET, which is often calibrated to match regional and plot characteristics [@OUDIN2005290].
 
The standard procedure to determine the potential evaporation is using the Penamn-Monteith formulation 
([allen1998crop, @walter2000asce)]), which requires standard meteorological measurements of air temperature, 
relative humidity, wind speed and radiation. Application of the Penamn-Monteith is therefore limited, as all of the
data is often not available. Alternative methods that require less meteorological input are therefore in favor in 
regions where all standard meteorological variables are not measured or climate scenarios, that only offer 
projections of radiation and temperature. The study of @OUDIN2005290 gives a great introduction on the importance 
of alternative methods to estimate PET and also reports that in some regions the alternative PET methods outperform 
the Penman-Monteith method.

# Functionality

At this stage, `PyEt` offers the estimation of daily PET using 20 different PET methods (see table below). 
The package also provides numerous functions to estimate missing meteorological data (e.g. solar and net radiation).
The methods are currently only implemented for 1D data (time series data). Future work will therefore centre on  
expanding functionality on 2D and 3D data as well (Numpy array, XArray, and NetCDF). The implemented methods are 
tested against other open source data to ensure proper functioning of the methods [@allen1998crop].

 Table 1: PET, surface and aerodynamic resistance methods included in `PyEt`. T, Temperature; U, Wind Speed; 
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

This example shows how `PyEt` can be used to compute potential evaporation using different evaporation 
methods. The potential evaporation is estimated for the city of De Bilt, using online available data 
provided by the The Royal Netherlands Meteorological Institute (KNMI). The potential evaporation is computed using 
four different methods (Penman, Priestley-Taylor, Makking, Oudin) and plotted for comparison.

``` python
# Import needed packages
import pandas as pd
import pyet

# Import meteorological data 
meteo = pd.read_csv("data/meteo_maribor_2017.csv", parse_dates=True, 
                    index_col="Date")
tmax, tmin, rh, wind, rs = [meteo[col] for col in meteo.columns]

# Compute Evaporation
et_penman = pyet.pm(wind, rs=rs, elevation=279, lat=0.813, tmax=tmax, 
					tmin=tmin, rh=rh)
et_pt = pyet.priestley_taylor(wind, rs=rs, elevation=279, lat=0.813, 
							  tmax=tmax, tmin=tmin, rh=rh)
et_mak = pyet.makkink(rs, tmax=tmax, tmin=tmin, elevation=279)
et_hamon = pyet.hamon(tmean.index, tmean, lat=0.813)
```
In the code above `R_s` is the incoming solar radiation [MJ m-2 d-1], `elevation` the site elevation [m], 
`lat` the the site latitude [rad], `tmax` and `tmin` the maximum and minimum temperature [°C] and 
`rh` the mean relative humidity [%].

![Daily potential evaporation for Maribor (Slovenia) estimated according to [@monteith1965evaporation], 
[@priestley1972assessment], [@makkink1957testing] and [@hamon1963estimating].](Figure1.png)


# Concluding remarks

This paper presents `PyEt`, a Python package to estimate daily potential evaporation from available 
meteorological data. Using `PyEt`, users can estimate PET using 20 different methods with only a few lines
of Python-code. At this stage (PyEt v1.0), the methods are implemented for 1D data (e.g. time series data).
Further developments will focus on enabling 2D and 3D data input (Numpy Arrays, Array, and NetCDF files).
The authors believe that `PyEt` is a valuable contribution to the hydrology, meteorology and agricultural 
communities, enabling a simple PET estimation and comparison between different PET estimates. `PyEt` methods 
and estimates can be further used in hydrological models and sensitivity analyses. The `PyEt` design enables
simple extension of the software with new capabilities and new model options. The authors warmly welcome 
code contributions, bug reports, and feedback from the community to further improve the package.
`PyEt` is free and open-source software under the LGPL-3.0 license and is available at
http://www.github.com/phydrus/PyEt. Full documentation is available on ReadTheDocs (https://pyet.readthedocs.io). 
The notebook and data necessary to reproduce the Figure in this manuscript are available through PyEt GitHub page.

# Acknowledgements

The first author is partly funded by the Earth System Sciences programme of the Austrian Academy of 
Sciences (project ClimGrassHydro). The second author is funded by the Austrian Science Fund (FWF) under Research 
Grant W1256 (Doctoral Programme Climate 525 Change: Uncertainties, Thresholds and Coping Strategies).

# References