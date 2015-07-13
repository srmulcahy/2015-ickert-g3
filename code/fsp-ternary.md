# Chemical and Pb-isotope composition of phenocrysts from bentonites constrain the chronostratigraphy around the Cretaceous-Paleogene boundary in the Hell Creek region, Montana

*Ryan B. Ickert, Sean R. Mulcahy, Courtney J. Sprain, Jessica F. Banaszak, Paul R. Renne*

Accepted for publication in G3.

## Feldspar ternary compositions in Figure 4.
```R
# Load necessary libraries
library(robCompositions)
library(RColorBrewer)
library(scales)

# Load data file
fsp <- read.table('../data/feldspar.csv', header = T, sep = ",")

# Subset data for plotting
end_mbrs <- fsp[, c(1:2, 51:54)]

# Subset by sample and coal
smpl <- levels(end_mbrs$Sample)
coal <- levels(end_mbrs$Coal)

# Define the color pallette
colourCount <- 13
getPalette <- colorRampPalette(brewer.pal(9, "Spectral"))
colors <- getPalette(colourCount)
palette(colors)

# Plot compositions on ternary diagrams
par(mfrow = c(4, 3))

# Subset tephra samples by coal
for(i in 1:length(coal)){
  a <- subset(end_mbrs, end_mbrs$Coal != coal[i])
  w <- subset(end_mbrs, end_mbrs$Coal == coal[i])
  s <- w$Sample
  b <- data.frame(a$Ab, a$Or, (a$An + a$Cn))
  z <- data.frame(w$Ab, w$Or, (w$An + w$Cn))
  names(b) <- c("Ab", "Or", "An+Cn")
  names(z) <- c("Ab", "Or", "An+Cn")
  grp <- as.integer(factor(s))

  # Plot samples not in coal[i] horizon as grey
  ternaryDiag(b, cex = 1.7,
              grid = FALSE,
              main = paste(coal[i], "-Coal Tephra", sep = ""),
              pch = 19,
              col = "lightgrey", #alpha("grey", 0.2),
              mcex = 0.75)
  # Plot each tephra in coal[i] horizon as a unique color
  ternaryDiagPoints(z, cex = 1.7,
                    pch = 21,
                    col = grp,
                    bg = alpha(grp, 0.2))
  # Plot the legend
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
