# Chemical and Pb-isotope composition of phenocrysts from bentonites constrain the chronostratigraphy around the Cretaceous-Paleogene boundary in the Hell Creek region, Montana

*Ryan B. Ickert, Sean R. Mulcahy, Courtney J. Sprain, Jessica F. Banaszak, Paul R. Renne*

Accepted for publication in G3.

## Titanite ternary compositions in Figure 4.

```R
# load necessary libraries
library(robCompositions)
library(RColorBrewer)
library(scales)

# Define the color pallette
colourCount <- 12
getPalette <- colorRampPalette(brewer.pal(9, "Spectral"))
colors <- getPalette(colourCount)

#colors <- brewer.pal(12, "Paired")
palette(colors)

ttn <- read.table('../data/titanite.csv', header = T, sep = ",")


fac <- data.frame(Sample = ttn$Sample, Coal = ttn$Coal,
                  X = ttn$Fe3,
                  Y = ttn$Al,
                  Z = ttn$Ce+ttn$Y)

# Subset by sample and coal
smpl <- levels(ttn$Sample)
coal <- levels(ttn$Coal)

# Plot compositions on ternary diagrams by coal and sample
par(mfrow = c(4, 2))

for(i in 1:length(coal)){
  a <- subset(fac, fac$Coal != coal[i])
  w <- subset(fac, fac$Coal == coal[i])
  s <- w$Sample
  grp <- as.integer(factor(s))

  # plot the points by coal tephra, colored by sample, other coal tephras in grey

  ternaryDiag(a[, 3:5], cex = 1.7,
              grid = FALSE,
              main = paste(coal[i], " - Coal Tephra", sep = ""),
              pch = 19,
              col = alpha("grey", 0.2),
              mcex = 0.75,
              name = c("Fe3", "Al", "Ce+Y"))

  ternaryDiagPoints(w[, 3:5], cex = 1.7,
                    pch = 21,
                    col = grp,
                    bg = alpha(grp, 0.2))

  if(length(unique(s)) > 6){
    legend("topleft", legend=unique(s), title = "Sample",title.adj = 0.25,
           pch = 21, cex = 0.75,
           col=colors[1:length(grp)],
           pt.bg = alpha(colors[1:length(grp)], 0.2),
           pt.cex = 1.25, ncol = 1,
           bty="n")
  } else {
    legend("topleft", legend=unique(s), title = "Sample",title.adj = 0.25,
           pch = 21, cex = 0.75,
           col=colors[1:length(grp)],
           pt.bg = alpha(colors[1:length(grp)], 0.2),
           pt.cex = 1.25,
           bty="n")
  }
}
```
