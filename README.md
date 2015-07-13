# Chemical and Pb-isotope composition of phenocrysts from bentonites constrain the chronostratigraphy around the Cretaceous-Paleogene boundary in the Hell Creek region, Montana

*Ryan B. Ickert, Sean R. Mulcahy, Courtney J. Sprain, Jessica F. Banaszak, Paul R. Renne*

Manuscript accepted for publication in Geochemistry, Geophysics, Geosystems.
This repository contains electron probe microanalysis (EPMA) data and median Pb
isotopes and the necessary code to produce the compositional diagrams in Figures
4 and 5 of the manuscript.

## Project Folders

- [code](https://github.com/srmulcahy/2015-ickert-g3/tree/master/code): r scripts for generating plots
    - **fsp-ternary.md**: Plot ternary feldspar compositions in Figure 4
    - **ttn-ternary.md**: Plot ternary titanite compositions in Figure 4
    - **mds-plot.md**: Plot MDS diagram in Figure 5

- [data](https://github.com/srmulcahy/2015-ickert-g3/tree/master/data): raw data used in analysis
    - **feldspar.csv**: feldspar weight% oxides and formulas
    - **mds-feldspar.csv**: feldspar compositional data adjusted for mds
    - **pb-feldspar.csv**: median pb-isotope compositions
    - **titanite.csv**: titanite weight% oxides and formulas

## R Packages

[compositions](http://cran.r-project.org/web/packages/compositions/index.html) van den Boogaart, K.G., Tolosana, R., and Bren, M., 2014, compositions: Compositional Data Analysis. R package version 1.40-1. http://CRAN.R-project.org/package=compositions

[plyr](http://cran.r-project.org/web/packages/plyr/index.html) Wickham, H., 2011, The Split-Apply-Combine Strategy for Data Analysis. Journal of Statistical Software, 40(1), 1-29.

[robCompositions](http://cran.r-project.org/web/packages/robCompositions/index.html) Templ ,M., Hron,K., and Filzmoser, P., 2011, robCompositions: an R-package for robust statistical
analysis of compositional data. In V. Pawlowsky-Glahn and A. Buccianti, editors, Compositional Data
Analysis. Theory and Applications, pp. 341-355, John Wiley & Sons, Chichester (UK)

[RColorBrewer](http://cran.r-project.org/web/packages/RColorBrewer/index.html) Neuwirth, E., 2014, RColorBrewer: ColorBrewer Palettes. R package version 1.1-2. http://CRAN.R-project.org/package=RColorBrewer

[scales](http://cran.r-project.org/web/packages/scales/index.html) Wickham, H., 2015, scales: Scale Functions for Visualization. R package version 0.2.5. http://CRAN.R-project.org/package=scales
