# R base plot tricks
R base plot tricks


## par()

```
par(mfrow=c(4,4),
    mar=c(0.2,0.25,0.2,0.25), # margins
    oma=c(5,2,2,2), # outer margins
    mgp=c(2,1,0), # The margin line (in mex units) for the axis title, axis labels and axis line.
    tcl=-0.25) # the length of tick marks as a fraction of the height of a line of text. The default value is -0.5
```

## plot a table (using legend)

```
plot.new()
atable<-matrix(LETTERS[1:9],3,3)
legend("center",ncol=3,
       legend=c(atable))
```

## space out overlapping points

```
gplots::space(xx, yy, s=???)
```

## axis()

```
axis(1, # placement
     cex.axis=0.6, # axis size
     tck=-0.02, # shorter ticks
     padj=-2) # move text closer to axis
```

## empty plots

#### plot.new()

```
plot.new()
```

#### plot

```
plot(c(-2,2),c(-1,1),type='n',axes=FALSE,ann=FALSE)
```

#### barplot

```
barplot(atable,xlab=NULL,ylab=NULL,xaxt="n",yaxt="n",space=0)
```

