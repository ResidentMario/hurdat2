# README

## Introduction

These files constitute a cleaned-up version of the United States National Oceanic and Atmospheric Administration
(NOAA) [HURDAT2 datasets](http://www.nhc.noaa.gov/data/#hurdat).

HURDAT2 is the current iteration of the NOAA's public exports of hurricane tracking data. The NOAA provides two files
 on their portal, one for the [Atlantic Ocean](http://www.nhc.noaa.gov/data/hurdat/hurdat2-1851-2015-070616.txt) and
 one for the [Pacific](http://www.nhc.noaa.gov/data/hurdat/hurdat2-nepac-1949-2015-050916.txt). These are provided in
  a mildly convoluted format, which has been cleaned up into the two proper CSV-standard files contained in this
  export: `./data/pacific_storms.csv` and `./data/atlantic_storms.csv`.

## Contents

* `./notebooks/01---Munging Pacific Data.ipynb` &mdash; This Jupyter notebook cleans up the Pacific data file and
outputs the `CSV` file.
* `./notebooks/02---Munging Atlantic Data.ipynb` &mdash; This Jupyter notebook cleans up the Atlantic data file and
outputs the `CSV` file.
* `./data/pacific_storms.csv` &mdash; The Pacific storms cleaned-up CSV file.
* `./data/atlantic_storms.csv` &mdash; The Atlantic storms cleaned-up CSV file.
* `./README.md` &mdash; This document.
* `environment.yml` &mdash; A `conda` description of the environment necessary to run the notebooks.
* `./figures/hurricane-noaa.jpg` &mdash; A splash image of a hurricane. Public domain via NOAA ([source](http://www.noaanews.noaa.gov/stories2005/s2438.htm)).

## Data Dictionary

The dataset consists of observations of storm intensity and characteristics taken at 6-hour intervals (while the
storm is actively being tracked), with storms recieving tracking located contiguously (so `FAUSTO` follows `ELIDA`
follows `DOUGLASS`, and so on).

For example, here are the first five lines of tracking information for Hurriance Sandy:

```
48055,AL182012,SANDY,2012-10-21 18:00:00,,LO,14.3,-77.4,25,1006,0,0,0,0,0,0,0,0,0,0,0,0
48056,AL182012,SANDY,2012-10-22 00:00:00,,LO,13.9,-77.8,25,1005,0,0,0,0,0,0,0,0,0,0,0,0
48057,AL182012,SANDY,2012-10-22 06:00:00,,LO,13.5,-78.2,25,1003,0,0,0,0,0,0,0,0,0,0,0,0
48058,AL182012,SANDY,2012-10-22 12:00:00,,TD,13.1,-78.6,30,1002,0,0,0,0,0,0,0,0,0,0,0,0
48059,AL182012,SANDY,2012-10-22 18:00:00,,TS,12.7,-78.7,35,1000,50,60,0,0,0,0,0,0,0,0,0,0
```

The columns are:

* `index` &mdash; The line number.
* `id` &mdash; The id assigned by the NOAA to the storm.
* `name` &mdash; The name of the storm. Often `UNNAMED`, as only significant storms recieve names, and hurricane names
only started to be assigned in the 1950s. Note that hurricanes in different years may recieve the same name: e.g.
hurricane `IRENE` appeared on the eastern seaborn in 1959, 1971, 1981, 1999, 2005, and 2011.
* `date` &mdash; The date of the record. This is provided in ISO format, e.g. `2014-09-25 18:00:00`.
* `record_identifier` &mdash; A letter assigned to the record, providing some additional context. The possibilities are:

```
C – Closest approach to a coast, not followed by a landfall
G – Genesis
I – An intensity peak in terms of both pressure and wind
L – Landfall (center of system crossing a coastline)
P – Minimum in central pressure
R – Provides additional detail on the intensity of the cyclone when rapid changes are underway
S – Change of status of the system
T – Provides additional detail on the track (position) of the cyclone
W – Maximum sustained wind speed
```

* `status_of_system` &mdash; The type of storm. Options are:

```
TD – Tropical cyclone of tropical depression intensity (< 34 knots)
TS – Tropical cyclone of tropical storm intensity (34-63 knots)
HU – Tropical cyclone of hurricane intensity (> 64 knots)
EX – Extratropical cyclone (of any intensity)
SD – Subtropical cyclone of subtropical depression intensity (< 34 knots)
SS – Subtropical cyclone of subtropical storm intensity (> 34 knots)
LO – A low that is neither a tropical cyclone, a subtropical cyclone, nor an extratropical cyclone (of any intensity)
WV – Tropical Wave (of any intensity)
DB – Disturbance (of any intensity)
```

* `latitude` &mdash; The latitude (`y` positional component).
* `longitude` &mdash; The longitude (`x` positional component).
* `maximum_sustained_wind_knots` &mdash;  The maximum 1-min average wind associated with the tropical cyclone at an
elevation of 10 m with an unobstructed exposure, in knots (kt).
* `maximum_pressure` &mdash; The [central atmospheric pressure](https://en.wikipedia.org/wiki/Atmospheric_pressure)
of the hurricane.
* `34_kt_ne`
* `34_kt_se`
* `34_kt_sw`
* `34_kt_nw`
* `50_kt_ne`
* `50_kt_se`
* `50_kt_sw`
* `50_kt_nw`
* `64_kt_ne`
* `64_kt_se`
* `64_kt_sw`
* `64_kt_nw` &mdash; This entry and those above it together indicate the boundaries of the storm's
[radius of maximum wind](https://en.wikipedia.org/wiki/Radius_of_maximum_wind). 34 knots is considered tropical storm
 force winds, 50 knots is considered storm force winds, and 64 knots is considered hurricane force winds ([source](http://www.nhc.noaa.gov/help/tcm.shtml?WINDWAVERADII#EYESIZE)).
 These measurements provide the distance (in [nautical miles](https://en.wikipedia.org/wiki/Nautical_mile)) from the
 eye of the storm (its `latitude`, `longitude` entry) in which winds of the given force can be expected. This
 information is only available for observations since 2004.

The [original data description](http://www.nhc.noaa.gov/data/hurdat/hurdat2-format-atlantic.pdf) the NOAA publishes
provides the following additional notes:

* Cyclone number: In HURDAT2, the order cyclones appear in the file is determined by the date/time of the first
tropical or subtropical cyclone record in the best
track. This sequence may or may not correspond to the ATCF cyclone number. For example, the 2011 unnamed tropical storm AL20 which formed on 1
September, is sequenced here between AL12 (Katia – formed on 29 Aug) and AL13 (Lee – formed on 2 September). This mismatch between ATCF cyclone
number and the HURDAT2 sequencing can occur if post-storm analysis alters the relative genesis times between two cyclones. In addition, in 2011 it became
practice to assign operationally unnamed cyclones ATCF numbers from the end of the list, rather than insert them in sequence and alter the ATCF numbers of
cyclones previously assigned.

* Name: Tropical cyclones were not formally named before 1950 and are thus referred to as “UNNAMED” in the database.
Systems that were added into the
database after the season (such as AL20 in 2011) also are considered “UNNAMED”. Non-developing tropical depressions formally were given names (actually
numbers, such as “TEN”) that were included into the ATCF b-decks starting in 2003. Non-developing tropical depressions before this year are also referred to as
“UNNAMED”.

* Record identifier: This code is used to identify records that correspond to landfalls or to indicate the reason for inclusion of a record not at the standard synoptic
times (0000, 0600, 1200, and 1800 UTC). For the years 1851-1955, 1969’s Camille, and 1991 onward, all continental United States landfalls are marked, while
international landfalls are only marked from 1951 to 1955 and 1991 onward. The landfall identifier (L) is the only identifier that will appear with a standard
synoptic time record. The remaining identifiers (see table above) are only used with asynoptic records to indicate the reason for their inclusion. Inclusion of
asynoptic data is at the discretion of the Hurricane Specialist who performed the post-storm analysis; standards for inclusion or non-inclusion have varied over
time. Identification of asynoptic peaks in intensity (either wind or pressure) may represent either system’s lifetime peak or a secondary peak

* Time: Nearly all HURDAT2 records correspond to the synoptic times of 0000, 0600, 1200, and 1800. Recording best track data to the nearest minute became
available within the b-decks beginning in 1991 and some tropical cyclones since that year have the landfall best track to the nearest minute.

* Status: Tropical cyclones with an ending tropical depression status (the dissipating stage) were first used in the best track beginning in 1871, primarily for
systems weakening over land. Tropical cyclones with beginning tropical depression (the formation stage) were first included in the best track beginning in 1882.
Subtropical depression and subtropical storm status were first used beginning in 1968 at the advent of routine satellite imagery for the Atlantic basin. The low
status – first used in 1987 - is for cyclones that are not tropical cyclone or subtropical cyclones, nor extratropical cyclones. These typically are assigned at the
beginning of a system’s lifecycle and/or at the end of a system’s lifecycle. The tropical wave status – first used in 1981 - is almost exclusively for cyclones that
degenerate into an open trough for a time, but then redevelop later in time into a tropical cyclone (for example, AL10-DENNIS in 1981 between 13 and 15
August). The disturbance status is similar to tropical wave and was first used in 1980. It should be noted that for tropical wave and disturbance status the location
given is the approximate position of the lower tropospheric vorticity center, as the surface center no longer exists for these stages.

* Maximum sustained surface wind: This is defined as the maximum 1-min average wind associated with the tropical cyclone at an elevation of 10 m with an
unobstructed exposure. Values are given to the nearest 10 kt for the years 1851 through 1885 and to the nearest 5 kt from 1886 onward. A value is assigned for
every cyclone at every best track time. Note that the non-developing tropical depressions of 1967 did not have intensities assigned to them in the b-decks. These
are indicated as “-99” currently, but will be revised and assigned an intensity when the Atlantic hurricane database reanalysis project (Hagen et al. 2012) reaches
that hurricane season.

* Central Pressure: These values are given to the nearest millibar. Originally, central pressure best track values were only included if there was a specific
observation that could be used explicitly. Missing central pressure values are noted as “-999”. Beginning in 1979, central pressures have been analyzed and
included for every best track entry, even if there was not a specific in-situ measurement available.

* Wind Radii – These values have been best tracked since 2004 and are thus available here from that year forward with a resolution to the nearest 5 nm. Best
tracks of the wind radii have not been done before 2004 and are listed as “-999” to denote missing data. Note that occasionally when there is a non-synoptic time
best track entry included for either landfall or peak intensity, that the wind radii best tracks were not provided. These instances are also denoted with a “-999” in
the database.

* General Notes: The database goes back to 1851, but it is far from being complete and accurate for the entire century and a half. Uncertainty estimates of the best track parameters available for are available for various era
in Landsea et al. (2012), Hagen et al. (2012), Torn and Snyder (2012), and Landsea and Franklin (2013). Moreover, as
one goes back further in time in addition to larger uncertainties, biases become more pronounced as well with tropical cyclone frequencies being underreported and
the tropical cyclone intensities being underanalyzed. That is, some storms were missed and many intensities are too low in the pre-aircraft reconnaissance era
(1944 for the western half of the basin) and in the pre-satellite era (late-1960s for the entire basin). Even in the last decade or two, new technologies affect the best
tracks in a non-trivial way because of our generally improving ability to observe the frequency, intensity, and size of tropical cyclones. See Vecchi and Knutson
(2008), Landsea et al. (2010), Vecchi and Knutson (2012), Uhlhorn and Nolan (2012) on methods that have been determined to address some of the undersampling
issues that arise in monitoring these mesoscale, oceanic phenomenon.

## Building

To build these datasets yourself, you will need to have `python` installed, `jupyter` installed, and access to the
`pandas` and `requests` modules. Navigate to the `notebooks` folder and run the following in the command line:

    >>> jupyter nbconvert --to notebook "01---Munging Pacific Data.ipynb" --output "01---Munging Pacific Data.ipynb"
    >>> jupyter nbconvert --to notebook "02---Munging Atlantic Data.ipynb" --output "02---Munging Atlantic Data.ipynb"

This executes the notebooks in-place and emits the data files.

Alternatively, if something breaks or you want to explore what's happening, you should run `jupyter notebook` and
open the notebooks themselves.

## License

[MIT]

Copyright (c) 2016 Aleksey Bilogur

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
