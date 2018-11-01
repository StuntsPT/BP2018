### Class #4

#### Bioinformática Prática 2018

<img src="C01_assets/logo-FCUL.png" style="background:none; border:none; box-shadow:none;">

<center>Francisco Pina Martins</center>

<center>[@FPinaMartins](https://twitter.com/FPinaMartins)</center>

---

## Plotting

---

## How does it work?

<ul>
<li class="fragment">Plotting in R uses a function called `plot()`</li>
  <ul>
  <li class="fragment">There are others, but let's start simple</li>
  </ul>
<li class="fragment">The plot proprieties can either be provided at call time, or added *a posteriori*</li>
<li class="fragment">It is not as confusing as it sounds</li>
</ul>

---

## Rules of engagement

<ul>
<li class="fragment">Lines with a `#` below are wither new, or changed</li>
  <ul>
  <li class="fragment">Pay special attention to them</li>
  </ul>
<li class="fragment">You should clear all plots between each slide</li>
  <ul>
  <li class="fragment">Use the broom icon <img src="C04_assets/broom.png" style="background:none; border:none; box-shadow:none;"> above the **plots** pane for that</li>
  </ul>
</ul>

<p class="fragment">Ready?</p>

---

## A simple dot plot

```R
charles = c(12, 11, 12, 12, 13)
# This is the class of the main professor at the institute

plot(charles)
# Use this function to draw your first plot
```

|||

## Add a line + Title

```R
charles = c(12, 11, 12, 12, 13)
plot(charles, type="o", col="blue", pch="x")
# Check the help for "type"
# "col" sets the colour
# Also check "pch" for dot types

title(main="Students at the institute", col.main="#DAA520", font.main=8)
# This command will **alter** the plot - what happens if you don't clear the buffer?
# Another way to set the colour!
```

|||

## Add another class

```R
charles = c(12, 11, 12, 12, 13)
scott = c(2, 3, 3, 15, 15)
# Adds "professor" Scott class data
plot(charles, type="o", col="gray", ylim=c(0,15), pch="x")
# Let's add new limits (try without doing this too)

lines(scott, type="o", pch=22, lty=2, col="red")
# Plots Scott's class
# Check out what "lty" does

title(main="Students at the institute", col.main="#DAA520", font.main=8)
```

|||

## Automate the limits and label axes

```R
charles = c(12, 11, 12, 12, 13)
scott = c(2, 3, 3, 15, 15)

st_range = range(0, charles, scott)
# Range function! Let's automate the y-axis

plot(charles, type="o", col="gray", ylim=st_range, pch="x")
# Now we have automated limits

lines(scott, type="o", pch=22, lty=2, col="red")
title(main="Students at the institute", col.main="#DAA520", font.main=8)

title(xlab="Days", col.lab="darkgreen")
# Adds a label to the X axis
title(ylab="Students", col.lab="darkgreen")
# Adds a label to the Y axis
# These will mess up the names! Noooo!
```

|||

## Cleanup

```R
charles = c(12, 11, 12, 12, 13)
scott = c(2, 3, 3, 15, 15)

st_range = range(0, charles, scott)

plot(charles, type="o", col="gray", ylim=st_range, pch="x", ann=FALSE)
# This change removes the automatic "lables"

lines(scott, type="o", pch=22, lty=2, col="red")
title(main="Students at the institute", col.main="#DAA520", font.main=8)

title(xlab="Days", col.lab="darkgreen")
title(ylab="Students", col.lab="darkgreen")
```

|||

## Adding a legend

```R
charles = c(12, 11, 12, 12, 13)
scott = c(2, 3, 3, 15, 15)

st_range = range(0, charles, scott)

plot(charles, type="o", col="gray", ylim=st_range, pch="x", ann=FALSE)

lines(scott, type="o", pch=22, lty=2, col="red")
title(main="Students at the institute", col.main="#DAA520", font.main=8)

title(xlab="Days", col.lab="darkgreen")
title(ylab="Students", col.lab="darkgreen")

legend(1, st_range[2], c("charles","scott"),
       cex=0.8, 
              col=c("gray","red"), pch=4:22, lty=1:2);
	      # Adds a legend. What are the first 2 arguments?
	      # What does "cex" do?
	      # Check the "pch" numbers too
```

|||

## New axes

```R
charles = c(12, 11, 12, 12, 13)
scott = c(2, 3, 3, 15, 15)

st_range = range(0, charles, scott)

plot(charles, type="o", col="gray", ylim=st_range, pch="x", ann=FALSE, axes=FALSE)
# Remove the axes. Careful!

axis(1, at=1:5, lab=c("Mon","Tue","Wed","Thu","Fri"))
# Adds an X axis (1).
# What do "at" and "lab" do?

axis(2, at=4*0:st_range[2])
# Adds an Y axis (2)
# Take a better look at "at" 

lines(scott, type="o", pch=22, lty=2, col="red")
title(main="Students at the institute", col.main="#DAA520", font.main=8)

title(xlab="Days", col.lab="darkgreen")
title(ylab="Students", col.lab="darkgreen")

legend(1, st_range[2], c("charles","scott"),
       cex=0.8, 
              col=c("gray","red"), pch=4:22, lty=1:2)
```

|||

## Getting data from an external file

```R
classes_data = read.csv("https://gitlab.com/StuntsPT/bp2018/raw/master/docs/classes/C04_assets/classes_data.txt", header=TRUE, sep="\t")
# Now we load the data from an external source

max_y = max(classes_data)
# Get maximum value to adjust axes to

plot_colors <- c("gray","red","orange")
# Define plot colours previously to avoid large trains of code later on

plot(classes_data$Charles, type="o", col=plot_colors[1], pch="x",
     ylim=c(0,max_y), axes=FALSE, ann=FALSE)
# We also have to adjust our plotting function

axis(1, at=1:5, lab=c("Mon","Tue","Wed","Thu","Fri"))
axis(2, at=4*0:max_y)
# We also had to change the Y-axis to use the new variable

lines(classes_data$Scott, type="o", pch=22, lty=2,
      col=plot_colors[2])
# The line for "Scott" also needs to refer to the new data source

lines(classes_data$Logan, type="o", pch=14, lty=6, 
      col=plot_colors[3])
# There is new data, this time for "professor" Logan

title(main="Students at the institute", col.main="#DAA520", font.main=8)
title(xlab="Days", col.lab="darkgreen")
title(ylab="Students", col.lab="darkgreen")

legend(1, max_y, names(classes_data), cex=0.8, col=plot_colors, 
       pch=c(4, 22, 14), lty=c(1,2,6));
# The code to set the legend is also changed. can you tell why?
# Maybe now it should be placed somewhere else...

```

[Alternate link](https://raw.githubusercontent.com/StuntsPT/BP2018/master/docs/classes/C04_assets/classes_data.txt)

|||

## "Saving the plot"

```R
classes_data = read.csv("https://gitlab.com/StuntsPT/bp2018/raw/master/docs/classes/C04_assets/classes_data.txt", header=TRUE, sep="\t")

max_y = max(classes_data)
plot_colors <- c("gray","red","orange")

png(filename="C:\figure.png", height=295, width=300, 
    bg="white")
# In order to save a plot we first need to tell R some information
# Can you tell what each option does?

plot(classes_data$Charles, type="o", col=plot_colors[1], pch="x",
     ylim=c(0,max_y), axes=FALSE, ann=FALSE)

axis(1, at=1:5, lab=c("Mon","Tue","Wed","Thu","Fri"))
axis(2, at=4*0:max_y)

lines(classes_data$Scott, type="o", pch=22, lty=2,
      col=plot_colors[2])

lines(classes_data$Logan, type="o", pch=14, lty=6, 
      col=plot_colors[3])

title(main="Students at the institute", col.main="#DAA520", font.main=8)

title(xlab="Days", col.lab="darkgreen")
title(ylab="Students", col.lab="darkgreen")

legend(1, max_y, names(classes_data), cex=0.8, col=plot_colors, 
       pch=c(4, 22, 14), lty=c(1,2,6));

dev.off()
# Now that our plot is complete, we just have to 
# "turn off" the device driver (which flushes output to our png image)
```

---

## Summary

|||

## Summary

<ul>
  <li class⁼"fragment">`plot()` will start a new plot in R</li>
  <ul>
    <li class="fragment">`dev_off()` will end it</li>
    <li class="fragment">We can keep adding to it until we finish it</li>
  </ul>
  <li class="fragment">`lines()` will allow us to plot additional lines</li>
  <li class="fragment">`title()` allows for changing the title, subtitle and add axis lables</li>
  <li class="fragment">`axis()` allows for axes manipulations</li>
  <li class="fragment">`legend()` adds a legend to the plot</li>
</ul>
