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

## a plot with only text

Method 1
```
plot.new()
legend("center",legend="Yes",lty=NULL,pch=NULL,ncol=1,
       xjust=0.5,
       yjust=0.5,
       x.intersp=-0.3)
```

Method 2
```
plot(c(-2,2),c(-1,1),type='n',axes=FALSE,ann=FALSE)
text(0,0,"Yes")
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

## add column/row titles

https://github.com/youyugithub/imitate_lattice_xyplot_using_base_plot_in_R

## Visualize text categories

```
visualize_text<-function(mytext,mysize,myscale=0.95){
  plot(c(-50,50),c(0,100),type="n",axes=FALSE,ann=FALSE)
  mystrheight<-rep(NA,length(mytext))
  for(ii in 1:length(mytext)){
    if(mysize[ii]>0)
      mystrheight[ii]<-strheight(mytext[ii],cex=100*mysize[ii]/sum(mysize))
  }
  mycex<-100*myscale*mysize/sum(mysize)*100/sum(mystrheight,na.rm=T)
  mybottom<-100-100*cumsum(mysize)/sum(mysize)
  mytop<-c(100,mybottom[1:(length(mybottom)-1)])
  mycenter<-(mybottom+mytop)/2
  for(ii in 1:length(mytext)){
    if(mysize[ii]>0)
      text(0,mycenter[ii],labels=mytext[ii],cex=mycex[ii])
  }
}
```
