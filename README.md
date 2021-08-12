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
# Change panels

```
# https://stackoverflow.com/questions/17410951/change-plot-panel-in-multipanel-plot-in-r

pars <- c('plt','usr')

par(mfrow=c(2,2))

plot(anscombe$x1, anscombe$y1, type='n')
par1 <- c(list(mfg=c(1,1,2,2)), par(pars))
plot(anscombe$x2, anscombe$y2, type='n')
par2 <- c(list(mfg=c(1,2,2,2)), par(pars))
plot(anscombe$x3, anscombe$y3, type='n')
par3 <- c(list(mfg=c(2,1,2,2)), par(pars))
plot(anscombe$x4, anscombe$y4, type='n')
par4 <- c(list(mfg=c(2,2,2,2)), par(pars))

for( i in 1:11 ) {
  par(par1)
  points(anscombe$x1[i], anscombe$y1[i])
  Sys.sleep(0.5)
  par(par2)
  points(anscombe$x2[i], anscombe$y2[i])
  Sys.sleep(0.5)
  par(par3)
  points(anscombe$x3[i], anscombe$y3[i])
  Sys.sleep(0.5)
  par(par4)
  points(anscombe$x4[i], anscombe$y4[i])
  Sys.sleep(0.5)
}



par(mfrow=c(2,2))
plot(rnorm(10), rnorm(10))
tmp <- par('usr')
hist(rgamma(1000,3)) # changes coordinate system
par(mfg=c(1,1)) # go back to first plot
points(0,0, col='red')  # wrong place, based on hist coordinate system
par(usr=tmp)  # reset coordinates to correct values
points(0,0, col='blue')  # now it is in the right place

```

```
color_levels<-function(x){
  if(is.null(levels(x)))return(NULL)
  x_levels<-levels(x)
  n_levels<-length(x_levels)
  x_hues<-seq(15,375,length=n_levels+1)
  x_colors<-hcl(h=x_hues,l=65,c=100)[1:n_levels]
  names(x_colors)<-x_levels
  x_colors
}
```

# Legend tricks

```
## no border
bty="n"
## gray border
border="gray"
```

# axis tricks
```
axis(...,cex.axis=0.75,padj=-2,tck=-0.05)
```

# axis rotation/rotate

The las parameter takes integers in the set {0,1,2,3}. The two we care about here are 0, which specifies axis labels should always be parallel to the axis (the default argument), and 2, which causes labels to always be drawn perpendicular to the axis. To make the y-axis labels easier to read, we want them to be perpendicular to the axis, so we will pass las = 2 to the axis function. (Ref: https://www.tenderisthebyte.com/blog/2019/04/25/rotating-axis-labels-in-r/).

#

pdf("output/1ab_tree_clusters_characteristics.pdf",width=7,height=6)

layout_matrix<-matrix(1:(nclus*6),nclus,6,byrow=TRUE)
layout(layout_matrix)

par(#mfrow=c(nclus,7),
  mar=c(0.2,0.25,0.2,0.25),
  oma=c(7,2,2,2),
  mgp=c(2,1,0),
  tcl=-0.25)

for(idx_rank in 1:nclus){
  df_subset<-df_merge%>%filter(rank==idx_rank)
  n_subset<-nrow(df_subset)
  
  plot(c(-2,2),c(-1,1),type='n',axes=FALSE,ann=FALSE)
  legend("center",legend=c(paste("Cluster",idx_rank),paste("n =",n_subset)),bty="n",ncol=1,cex=0.8)
  
  hist(df_subset$Age,main="",xlab=NULL,ylab=NULL,xaxt="n",yaxt="n",
       breaks=seq(range_age[1]-0.5,range_age[2]+0.5,length.out=11))
  if(idx_rank==1)add_col_title(mytitle="Age")
  if(idx_rank==nclus)axis(1,cex.axis=0.6,tck=-0.02,padj=-2)
  
  barplot(table(df_subset$AAB)/n_subset,xlab=NULL,ylab=NULL,xaxt="n",yaxt="n",space=0)
  if(idx_rank==1)add_col_title(mytitle="AB")
  if(idx_rank==nclus)
    axis(side=1,at=seq_along(table(df_subset$AAB))-0.5,
         tick=FALSE,labels=levels(df_subset$AAB),las=3,
         cex.axis=0.8)
  
  barplot(table(df_subset$Sibling)/n_subset,xlab=NULL,ylab=NULL,xaxt="n",yaxt="n",space=0)
  if(idx_rank==1)add_col_title(mytitle="Sibling")
  if(idx_rank==nclus)
    axis(side=1,at=seq_along(table(df_subset$Sibling))-0.5,
         tick=FALSE,labels=levels(df_subset$Sibling),las=3,
         cex.axis=0.8)
  
  barplot(table(df_subset$GRS2_grp)/n_subset,xlab=NULL,ylab=NULL,xaxt="n",yaxt="n",space=0)
  if(idx_rank==1)add_col_title(mytitle="GRS")
  if(idx_rank==nclus)axis(side=1,at=seq_along(table(df_subset$GRS2_grp))-0.5,
                          tick=FALSE,labels=levels(df_subset$GRS2_grp),las=3,
                          cex.axis=0.8)
  
  afit<-survfit(Surv(Survival_Time,T1D_Indicator==1)~1,data=df_subset)
  
  plot(afit,xlab="",ylab="",xaxt="n",yaxt="n")
  if(idx_rank==1)add_col_title(mytitle="T1D")
  if(idx_rank==nclus)
    axis(side=1,at=seq_along(table(df_subset$T1D_Indicator))-0.5,
         tick=FALSE,labels=levels(df_subset$T1D_Indicator),las=3,
         cex.axis=0.8)
  
}
dev.off()

##

```
plot(0,0)
text(0,0,"text",pos=1)
text(0,0,"text",pos=2)
text(0,0,"text",pos=3)
text(0,0,"text",pos=4)

plot(0,0)
text(0,0,"text",srt=90,pos=1)
text(0,0,"text",srt=90,pos=2)
text(0,0,"text",srt=90,pos=3)
text(0,0,"text",srt=90,pos=4)

plot(0,0)
text(0,0,"text",srt=0,pos=1)
text(0,0,"text",srt=90,pos=1)
text(0,0,"text",srt=180,pos=1)
text(0,0,"text",srt=270,pos=1)

plot(0,0)
text(0,0,"text",srt=0,pos=2)
text(0,0,"text",srt=90,pos=2)
text(0,0,"text",srt=180,pos=2)
text(0,0,"text",srt=270,pos=2)
```

## Adjust axis labels position

```
title(xlab="Years Since Treatment",line=2,cex.lab=0.75)
title(ylab="Subjects",line=1,cex.lab=0.75)
```

## save as jpg/jpeg
https://stackoverflow.com/questions/51192059/saving-high-resolution-plot-in-png
As you can read in ?jpeg you can use a filename with a "C integer format" and jpeg will create a new file for each plot, e.g.:
```
jpeg(filename="Rplot%03d.jpeg")
plot(1:10)  # will become Rplot001.jpeg
plot(11:20) # will become Rplot002.jpeg
dev.off()
```

```
library(pryr)
pryr_timecheck %<a-% {
    ...
}

pdf("timecheck.pdf",width=8,height=11)
pryr_timecheck
dev.off()

jpeg("timecheck%03d.pdf",width=120*8,height=120*11,quality=100,res=75*2)
pryr_timecheck
dev.off()
```

## change color gradually, gradient color,

```
col_sd<-circlize::colorRamp2(c(-2,0,2),c("blue","purple","red"))
```
