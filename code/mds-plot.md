# Chemical and Pb-isotope composition of phenocrysts from bentonites constrain the chronostratigraphy around the Cretaceous-Paleogene boundary in the Hell Creek region, Montana

*Ryan B. Ickert, Sean R. Mulcahy, Courtney J. Sprain, Jessica F. Banaszak, Paul R. Renne*

Accepted for publication in G3.

## Mutli-dimensional scaling (MDS) Figure 5.

```R
# Load necessary libraries
library(robCompositions)
library(compositions)
library(RColorBrewer)
library(scales)
library(plyr)

# Import the EPMA and Pb data
# ---
fsp <- read.table('../data/mds-feldspar.csv', header = T, sep = ",")
pb_avg <- read.table("../data/pb-feldspar.csv", header = TRUE, sep = ",")

# Convert to log-ratios
# ---
# Subset just the alklai feldspar
ox <-  fsp[, c(1:3, 5, 6, 9, 11:13)]
ox <- subset(ox, fsp$Or >= 0.40)

# Convert to log ratios
ox_ilr <- isomLR(ox[, -c(1:2)])

# Calculate mean compositions by sample and coal
# ---
ox_ilr2 <- as.data.frame(ox_ilr)
ox_ilr2$Sample <- ox$Sample
ox_ilr2$Coal <- ox$Coal

# Average log-ration composition
ox_avg <-  ddply(ox_ilr2, .(Sample, Coal), colwise(mean))
ox_std <-  ddply(ox_ilr2, .(Sample, Coal), colwise(sd))

# Calculate the metric variance of the two datasets
# ---
ox_mv <- mvar(ox_avg[, -c(1,2)])
pb_mv <- mvar(pb_avg[, -1])

# Downscale datasets by inverse metric variance
# ---
alpha = 1/sqrt(ox_mv)
beta = 1/sqrt(pb_mv)
ox_ds <- data.frame(ox_avg[, 1:2], alpha*ox_avg[, -c(1:2)])
pb_ds <- data.frame(Sample = pb_avg[, 1], Pb = beta * pb_avg[, 2])

df <- merge(ox_ds, pb_ds, by = "Sample")

# Uncomment to leave out HTC12-3 from plot
df <- df[-c(2, 18, 23, 33:35), ]

# Calculate pair-wise Aithchison distnances
# ---
diss2 <- dist(df[, -c(1:2)], method = "euclidean", diag = TRUE, upper = TRUE, p = 2)
diss2 <- as.matrix(diss2)
rownames(diss2) <- df[, 1]
colnames(diss2) <- df[, 1]

# Classic (metric) MDS
# ---
mds2 = cmdscale(diss2)

# Define the color pallette
colourCount <- 10
getPalette <- colorRampPalette(brewer.pal(9, "Spectral"))
colors <- getPalette(colourCount)
palette(colors)

# Plot the MDS results
# ---
# Function to plot MDS results
# Modfied from Vermeesch, P., 2013, Multi-sample comparison of detrital age
# distributions, Chemical Geology, v. 341, p. 140-146.

mds_plot <- function(conf, diss, clr, bgr, label = TRUE, title = "") {
  # rank the samples according to their pairwise proximity
  i = t(apply(as.matrix(diss), 1, function(x) order(x))[2:3, ])
  # plot coordinates for the lines
  x1 = as.vector(conf[i[, 1], 1])
  y1 = as.vector(conf[i[, 1], 2])
  x2 = as.vector(conf[i[, 2], 1])
  y2 = as.vector(conf[i[, 2], 2])
  # create a new (empty) plot
  plot(conf[, 1], conf[, 2],
       type= 'n',
       xlab = "MDS1 (Pb isotope composition)",
       ylab = "MDS2 (Chemical composition)",
       main = title)
  # grid()
  # draw lines between closest neighbours
  for (j in 1:nrow(conf)) {
    lines(c(conf[j, 1], x1[j]), c(conf[j, 2], y1[j]),
          lty=1,
          col = "lightgrey")
    lines(c(conf[j, 1], x2[j]), c(conf[j ,2], y2[j]),
          lty=2,
          col = "lightgrey")
  }
  # plot the configuration as labeled circles
  points(conf[, 1], conf[, 2],
         pch = 21,
         cex = 1.7,
         col = clr,
         bg = bgr)
  if(label == TRUE){
    text(conf[, 1], conf[, 2],
         row.names(conf),
         cex = 0.5)
  }
}
# Plot the figure
mds_plot(mds2, diss2,
         clr = as.numeric(df$Coal),
         bgr = alpha(as.numeric(df$Coal), 0.5),
         label = TRUE,
         title = "Figure 5 - Ickert et al., 2015")

legend("topleft", as.character(levels(df$Coal)),
       pch = 21, pt.cex = 1, cex=0.75,
       col = colors,
       pt.bg = alpha(colors, 0.5),
       ncol=2,
       bty="n")
```
