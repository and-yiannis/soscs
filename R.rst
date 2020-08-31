#
R
#

Most of the material is taken from https://r-statistics.co/


General
#######

Variable Types
****************
* character
* integer
* numeric
* factor
* logical
* complex

Data Types
***********

* vector
* matrix
* data.frame
* list

Set working directories
***********************

.. code-block:: r

   getwd()
   setwd()

Import export data
******************

.. code-block:: r

   myData <- read.table("data.txt")
   myData <- read.csv("data.csv")

Libraries
*********

.. code-block:: r

   install.packages("car") # install the package
   installed.packages() # list all the installed packages

   library(car) # initialise the package
   require(car) # alternative initialisation
   library() # see the list of all installed packages

   require(help=car) # See info about a package
   search() # list all the loaded packages

   data() # list the datasets in the loaded packages
   data("midwest", package="ggplot2") # load the data set in the global workspace


Execute system commands
***********************

:code:`system`

Read/write binary files
***********************

.. code-block:: r


   # Open file for reading 
   fid = file('filename.bin','rb')
   # Position the file pointer
   seek(fid, where=0, origin=c("start", "current", "end"))
   # Return the position of the pointer
   seek(fid)
   # Read from file where n is the number of elements (they can be read from 
   # the size of file)
   a = readBin(fid,double(),n)
   # Write to file 
   writeBin(X,fid,double())
   # Close the file 
   close(fid)

The paste function
******************

.. code-block:: r

   paste("a", "b")  # "a b"
   paste0("a", "b")  # concatenate without space, "ab"
   paste("a", "b", sep="")  # same as paste0
   paste(c(1:4), c(5:8), sep="")  # "15" "26" "37" "48"
   paste(c(1:4), c(5:8), sep="", collapse="")  # "15263748"
   paste0(c("var"), c(1:5))  # "var1" "var2" "var3" "var4" "var5"
   paste0(c("var", "pred"), c(1:3))  # "var1" "pred2" "var3" Extension!!
   paste0(c("var", "pred"), rep(1:3, each=2))  # "var1" "pred1" "var2" "pred2" "var3" "pred3

Dealing with dates
******************

.. code-block:: r

   dateString <- "15/06/2014"
   myDate <- as.Date(dateString, format="%d/%m/%Y")
   class(myDate)  # "Date"
   myPOSIXltDate <- as.POSIXlt(myDate)
   class(myPOSIXltDate)  # POSIXlt
   myPOSIXctDate <- as.POSIXct(myPOSIXltDate)  # convert to POSIXct

Object interrogation
********************

.. code-block:: r

   class(myPOSIXltDate)       # Get the class of an object
   attributes(myPOSIXltDate)  # Return a list of the object's attributes
   unclass(myPOSIXltDate) # Returns an object with the class attribute removed
   names(myPOSIXltDate)   # Get or set the names of an object
   unlist(myPOSIXltDate)  # Given a list structure x, unlist simplifies it to 
                          # produce a vector which contains all the atomic 
                          # components which occur in x.
   object.size(myPOSIXltDate)
   typeof(x) # Determine the type of storage mode of any object
   str(x) # Compactly display the structure of an arbitrary R object
   

Contingency table
*****************

.. code-block:: r

   table(airquality$Month[c(1:60)], airquality$Temp[c(1:60)]) # first 60/code>

Lists
*****

.. code-block:: r

   myList <- list(vec1, vec2)     # Between a python list and a dictionary
   myList <- list(a=vec1, b=vec2) 

   # Get the entire vector
   myList[1]     # $a 
   myList['a']   # [1] 10 20 15
   
   # Get the vector's elements only
   myList[[1]]     # 
   myList[['a']]   # [1] 10 20 15
   myList$a        #
   
   # Flatten out list
   unlist(myList) 


Apply functions
***************

.. code-block:: r

   apply(df, FUN=) # apply a function through a data frame, by rows or columns
   lapply(myList, length) # apply function length to each element of the list
   sapply(myList, length) # like lapply, but returns a vector
   vapply(myList, length) # like sapply, but faster if the return type is known

If else
*******

.. code-block:: r

   if(checkConditionIfTrue) {
     #....statements..
     #....statements..
   } else { 
     #....statements..
     #....statements..
   } 

If else vectorised
******************

.. code-block:: r

   a <- c(1, 2, 1)
   x = ifelse(a==1, 'a', 'b') #  [1] "a" "b" "a"
   x <- ifelse(a==2, 'a', 'b') #  [1] "b" "a" "b"
   class(x)

For loop
********

.. code-block:: r

   for(counterVar in c(1:n)){
     #.... statements..
   }





Vectors
#######

.. code-block:: r

   vec1 <- c(10, 20, 15, 40)
   vec2 <- c("a", "b", "c", NA)
   length(vec1)
   vec1[1:3]

Initialise
**********

.. code-block:: r

    vec <- numberic(100)

Subset
******

.. code-block:: r

   vec1[vec1<15] # Logic assignment
   vec1[1:2]
   vec1[c(1, 2)]
   vec1[-1] # All elements except for the last one


Sorting-Ordering
****************

.. code-block:: r

   sort(vec1)
   sort(vec1, decreasing=True)
   order(vec1)
   rev(order(vec1)) # reverse order

Sequences
*********

.. code-block:: r

   seq(1, 10, by=2)  # Take every other number from 1 to 10
   seq(1, 10, length=25) # Like linspace
   rep(1, 5) # Repeat 1 5 times
   rep(1:3, 5) # Repeat 1 to 3 5 times
   rep(1:3, each=5) # Repeat each of 1 to 3 5 times

Missing values
**************

.. code-block:: r

   is.na(vec2)
   !is.na(vec2)

Sampling 
********

.. code-block:: r

   sample(vec1) # sample all elements randomly (permutation)
   sample(vec1, 3) # sample 3 elements without replacement
   sample(vec1, 3, replace=T) # sample with replacement
 
Data frames
###########

.. code-block:: r

   myDf1 <- data.frame(vec1, vec2)
   library(datasets)
   library(help=datasets)
 
Creating dataframes
*******************

.. code-block:: r

    # Create a simple data frame
    df = data.frame('a'=c(1,2,3), 'b'=c(3,4,5))

    # Get and set the column names 
    names(df)
    names(df) <- c('col1', 'col2' )

    # Get and set the row names
    row.names(df)
    row.names(df) <- c('a', 'b', 'c')


Examining dataframes
********************

.. code-block:: r

   class(airquality)  # get class
   sapply(airquality, class)  # get class of all columns
   str(airquality)  # structure
   summary(airquality)  # summary of airquality
   head(airquality)  # view the first 6 observations
   tail(airquality)  # view the last 6 observations
   fix(airquality)  # view spreadsheet like grid
   rownames(airquality)  # row names
   colnames(airquality)  # columns names
   dimnames(airquality)  # both row and column names
   nrow(airquality)  # number of rows
   ncol(airquality)  # number of columns

Concatenate dataframes
**********************

.. code-block:: r

   cbind(df1, df2) # Concatenate columns
   rbind(df1, df2) # Concatenate rows


Subsetting dataframes
*********************

.. code-block:: r

   myDf1$vec1 # vec1 column
   myDf1[,1] # first column
   myDf1[, c(1,2)] # columns 1 and 2
   myDf1[c(1:5), c(2)] # first 5 rows in column 2

   subset(airquality, Day==1, select=-Temp) # keep rows where Day == 1 and all 
                                            # columns except for Temp
   airquality[which(airquality$Day==1), -c(4)] # Same as above, with which
   
Merging databases
*****************

.. code-block:: r

   merge(df1, df2, by= , by.x=, by.y=, all=, all.x=, all.y=)

Time series
###########

The ts package
**************

The time series class

.. code-block:: r

   ts(data = NA, start = 1, end = numeric(), frequency = 1,
      deltat = 1, ts.eps = getOption("ts.eps"), class = , names = )

The attributes of a ts variable can be extracted using the respective functions, e.g.

.. code-block:: r

   start(x) # Get the starting point
   end(x) # Get the starting point
   frequency(x) # Get the frequency

Extract a subset from a time series

.. code-block:: r

   window(x, start=..., end=...)

The xts package
***************

The xts package allows creating time series that have a date index, a little 
more like the pandas Series objects

.. code-block:: r

   # Create the object data using 5 random numbers
   data <- rnorm(5)
   
   # Create dates as a Date class object starting from 2016-01-01
   dates <- seq(as.Date("2016-01-01"), length = 5, by = "days")
   
   # Use xts() to create smith
   smith <- xts(x = data, order.by = dates)
   


GGplot
######

Simple scatter plot
*******************

Plotting with ggplot is based around data.frames. Below are shown different 
ways of making a scatter plot for :code:`midwest$area` vs :code:`midwest$poptotal`.

.. code-block:: r

   ggplot(midwest, aes(x=area, y=poptotal)) + geom_point()
   
   ggplot(midwest) + geom_point(aes(x=area, y=poptotal))

* :code:`ggplot` defines what data.frame will the plot be based on
* :code:`aes` (aesthetics) defines what data will the :code:`x` and :code:`y` coordinates will be based on
* :code:`geom_point` says that the plot will be a scatter plot (:code:`geom_line` would produce a line plot)
* The aesthetic can be placed either in the base call (:code:`ggplot`) or in the geometry call (:code:`geom_point`).

Geometry layers can be stacked onto each other as follows:

.. code-block:: r

   ggplot(midwest) +
       geom_point(aes(x=area, y=poptotal)) +
       geom_smooth(aes(x=area, y=poptotal), method='lm') 

This will produce a scatterplot and a smoothed loess curve of the data.

Time series plotting
********************

If x.train and x.test are both of class ts, the following will plot them both

.. code-block:: r

   autoplot(x) +
     autolayer(x.train, series="Training") +
     autolayer(x.test, series="Test")

Another way this can be done is with a combination of :code:`autoplot` and :code:`geom_line`.

.. code-block:: r

   fc1 <- ses(livestock, h=8)
   autoplot(livestock, colour='white', linetype='dotted', size=0) +
      geom_line(data=livestock, aes(y=livestock, col='livestock'), linetype='dashed', size=1) +
      geom_line(data=fc1$mean, aes(y=fc1$mean, col='fc1')) +
      scale_color_manual(values=c('red', 'black'), name='predictions')

.. image:: /images/R/TimeSeriesPlot1.png
    :scale: 100

The first line sets the scale, but we don't want to plot anything, it's just a way of creating a time series plot, as :code:`ggplot` doesn't accept :code:`ts` objects.

.. code-block:: r

   autoplot(livestock, colour='white', linetype='dotted', size=0)

The first line is created with the :code:`geom_line`

.. code-block:: r

   geom_line(data=livestock, aes(y=livestock, col='livestock'), linetype='dashed', size=1) +

Here we define where do the data come from (:code:`data=livestock`), and any further styles we want for this line, (:code:`linetype`, :code:`size`). The aesthetic :code:`aes`, determines the value of :code:`y` and gives a *name* to the colour. 

The final line, determines what colours will be actually used and gives a title to the legend.

.. code-block:: r

   scale_color_manual(values=c('red', 'black'), name='predictions')

Additional annotations can be added as follows


.. code-block:: r

   autoplot(livestock, colour='white', linetype='dotted', size=0) +
      geom_line(data=livestock, aes(y=livestock, col='livestock'), linetype='dashed', size=1) +
      geom_point(data=livestock, aes(y=livestock, col='livestock'), col='black', shape='+', size=5) +
      geom_line(data=fc1$mean, aes(y=fc1$mean, col='fc1')) +
      scale_color_manual(values=c('red', 'black'), name='predictions')

.. image:: /images/R/TimeSeriesPlot2.png
    :scale: 100

The following code will plot predictions from 2 different models, with their 95% confidence intervals.

.. code-block:: r

   fc1 <- ses(livestock, h=8)
   fc2 <- holt(livestock, h=8)
   
   autoplot(livestock) + 
     geom_line(aes(y=fc1$mean, col='ses'), data=fc1$mean) +
     geom_line(aes(y=fc2$mean, col='holt'), data=fc2$mean) +
     geom_ribbon(aes(ymin=fc1$lower[, '95%'],
                     ymax=fc1$upper[, '95%'],
                     fill="ses"),
                 data=fc1$mean, alpha=.1) +
     geom_ribbon(aes(ymin=fc2$lower[, '95%'],
                     ymax=fc2$upper[, '95%'],
                     fill="holt"),
                 data=fc2$mean, alpha=.1) +
     scale_color_manual(values=c("blue", "red"), name="") +
     scale_fill_manual(values=c("blue", "red"), name="")


.. image:: /images/R/TimeSeriesPlot3.png
    :scale: 100


Adjusting the x and y limits
****************************

.. code-block:: r


   g <- ggplot(midwest, aes(x=area, y=poptotal)) +
     geom_point() + geom_smooth(method="lm")
   
   # Zooms in by deleting the points outside the limits
   g0 <- g + xlim(c(0, 0.1)) + ylim(c(0, 1000000)) 
   plot(g0)
   
   # Zooms in without deleting the points
   g1 <- g + coord_cartesian(xlim=c(0,0.1), ylim=c(0, 1000000))
   plot(g1)


Change title and axes labels
****************************

.. code-block:: r

   # Make the base plot
   g <- ggplot(midwest, aes(x=area, y=poptotal)) +
     geom_point() + geom_smooth(method="lm") +
     coord_cartesian(xlim=c(0,0.1), ylim=c(0, 1000000))  # zooms in
   
   # Add Title and Labels
   g1 <- g + labs(title="Area Vs Population",
                  subtitle="From midwest dataset",
                  y="Population",
                  x="Area",
                  caption="Midwest Demographics")
   plot(g1)

   # alternatively...
   g2 <- g + ggtitle("Area Vs Population",
                     subtitle="From midwest dataset") +
     xlab("Area") + ylab("Population")
   plot(g2)

Change color and size of points
*******************************

.. code-block:: r
  
   # Set color and marker size statically 
   ggplot(midwest) + 
     geom_point(aes(x=area, y=poptotal), col="steelblue", size=3)
     
   # Vary colour depending on the value of midwest$state
   ggplot(midwest) + 
     geom_point(aes(x=area, y=poptotal, col=state), size=3) 
     
   # Static attributes will not work inside the aes, e.g. 
   aes(x=..., y=..., col="steelblue") # Will not work!!
   
   # Dynamic attributes will not work outside the aes
   geom_point(aes(x=area, y=poptotal), col=state) # ERROR!!


Also 

.. code-block:: r

   # Remove the legend
   gg + theme(legend.position="None")  # remove legend
   
   # Change the color palette.
   gg + scale_colour_brewer(palette = "Set1")  # change color palette

   # More palettes can be found in the RColorBrewer package
   library(RColorBrewer)


Ticks and location
******************

.. code-block:: r

   # Set the breaks every 0.01 and label with the first 11 letters. 
   ggplot(...) + 
      scale_x_continuous(breaks=seq(0, 0.1, 0.01), labels=letters[1:11])


Customise the labels

.. code-block:: r

   # Using sprintf
   ggplot(...) + scale_x_continuous(
      breaks=seq(0, 0.1, 0.01),
      labels = sprintf("%1.2f%%", seq(0, 0.1, 0.01))
   )

   # Using a custom function that takes as an argument the value of the break
   ggplot(...) + scale_y_continuous(
      breaks=seq(0, 1000000, 200000),
      labels = function(x){paste0(x/1000, 'K')}
   )


GGplot customisation
********************

Most of the requirements related to look and feel can be achieved using the 
:code:`theme()` function. It accepts a large number of arguments. Type 
:code:`?theme` in the R console and see for yourself.

The arguments passed to theme() components require to be set using special element_type() functions. They are of 4 major types.

*  :code:`element_text()`: Since the title, subtitle and captions are textual items, element_text() function is used to set it.
*  :code:`element_line()`: Likewise element_line() is use to modify line based components such as the axis lines, major and minor grid lines, etc.
*  :code:`element_rect()`: Modifies rectangle components such as plot and panel background.
*  :code:`element_blank()`: Turns off displaying the theme item.


# element_text(): Since the title, subtitle and captions are textual items, element_text() function is used to set it.
# element_line(): Likewise element_line() is use to modify line based components such as the axis lines, major and minor grid lines, etc.
# element_rect(): Modifies rectangle components such as plot and panel background.
# element_blank(): Turns off displaying the theme item.

Below is an example modifying the title and the subtitle of a plot

.. code-block:: r

   gg <- ggplot(midwest, aes(x=area, y=poptotal)) + 
     geom_point(aes(col=state, size=popdensity)) + 
     geom_smooth(method="loess", se=F) + xlim(c(0, 0.1)) + ylim(c(0, 500000)) + 
     labs(title="Area Vs Population", y="Population", x="Area", caption="Source: midwest")
   
   gg + theme(plot.title=element_text(size=20, 
                                      face="bold", 
                                      family="American Typewriter",
                                      color="tomato",
                                      hjust=0.5,
                                      lineheight=1.2),  # title
              plot.subtitle=element_text(size=15, 
                                         family="American Typewriter",
                                         face="bold",
                                         hjust=0.5),  # subtitle
              plot.caption=element_text(size=15),  # caption
              axis.title.x=element_text(vjust=10,  
                                        size=15),  # X axis title
              axis.title.y=element_text(size=15),  # Y axis title
              axis.text.x=element_text(size=10, 
                                       angle = 30,
                                       vjust=.5),  # X axis text
              axis.text.y=element_text(size=10))  # Y axis text

Below another plot changing the grids

.. code-block:: r


   g <- ggplot(mpg, aes(x=displ, y=hwy)) + 
         geom_point() + 
         geom_smooth(method="lm", se=FALSE) + 
         theme_bw()  # apply bw theme
   
   g + theme(panel.background = element_rect(fill = 'khaki'),
             panel.grid.major = element_line(colour = "burlywood", size=1.5),
             panel.grid.minor = element_line(colour = "tomato", 
                                             size=.25, 
                                             linetype = "dashed"),
             panel.border = element_blank(),
             axis.line.x = element_line(colour = "darkorange", 
                                        size=1.5, 
                                        lineend = "butt"),
             axis.line.y = element_line(colour = "darkorange", 
                                        size=1.5)) +
       labs(title="Modified Background", 
            subtitle="How to Change Major and Minor grid, Axis Lines, No Border")
   
   g + theme(plot.background=element_rect(fill="salmon"), 
             plot.margin = unit(c(2, 2, 1, 1), "cm")) +  # top, right, bottom, left
       labs(title="Modified Background", subtitle="How to Change Plot Margin")  

   







Forecasting
###########

This Chapter holds notes from the Forecasting: Principles and Practice book, by Rob J Hyndman and George Athanasopoulos.

Time series graphics
********************


Difference between seasonal and cyclic data
===========================================

* Seasonal data have fixed frequency
* Cyclic data have frequency that is not exact.
  
  

Simple time series plot
=======================

.. code-block:: r

   autoplot(melsyd[,"Economy.Class"]) +
     ggtitle("Economy class passengers: Melbourne-Sydney") +
     xlab("Year") +
     ylab("Thousands")
  
  
Seasonal plots
==============

.. code-block:: r

   ggseasonplot(a10, year.labels=TRUE, year.labels.left=TRUE) +
     ylab("$ million") +
     ggtitle("Seasonal plot: antidiabetic drug sales")
  
   ggseasonplot(a10, polar=TRUE) +
     ylab("$ million") +
     ggtitle("Polar seasonal plot: antidiabetic drug sales")  

Subseries plots
===============

.. code-block:: r

  ggsubseriesplot(a10) +
    ylab("$ million") +
    ggtitle("Seasonal subseries plot: antidiabetic drug sales")


Lag plots
=========

.. code-block:: r

  gglagplot(a10) 

Scatter plots
=============

.. code-block:: r

   qplot(Temperature, Demand, data=as.data.frame(elecdemand)) +
     ylab("Demand (GW)") + xlab("Temperature (Celsius)")

Scatter matrices
================

.. code-block:: r

   GGally::ggpairs(as.data.frame(visnights[,1:5]))


Autocorrelation plots
=====================

.. code-block:: r

   # Autocorrelation function
   ggAcf(x) # ggplot variant

   # Partial autocorrelation
   ggPacf(x)

   # Show both Acf and Pacf in a single plot
   ggtsdisplay(x)
  


The forecaster's toolbox
************************

Some Simple forecasting methods
===============================

**Mean**

All the forecasts equal the mean of the historical data

.. code-block:: r

   meanf(y, h)
   # y contains the time series
   # h is the forecast horizon


**Naive**

For naïve forecasts, we simply set all forecasts to be the value of the last
observation. 

.. code-block:: r

   naive(y, h)
   # Equivalent alternative
   rwf(y, h) 


**Seasonal Naive**

We set each forecast to be equal to the last observed value from the same 
season of the year 

.. code-block:: r

   snaive(y, h)


**Drift**

Equivalent to drawing a line between the first and last observations, and
extrapolating it into the future.

.. code-block:: r

   rwf(y, h, drift=TRUE)

Transformations and adjustments
===============================

**Box Cox transormations**

The Box-Cox transformation is given by

.. math:: 

   w_t &=& log(y_t)\  \textrm{if}\  \lambda=0 \\
   w_t &=& (y_t^\lambda - 1)/\lambda\  \textrm{otherwise}

The inverse transformation is 

.. math:: 

   y_t &=&  \exp(w_t) \ \textrm{if}\  \lambda = 0 \\
   y_t &=& (\lambda w_t + 1)^{1/\lambda} \ \textrm{otherwise} \\


.. code-block:: r

   # Estimate lambda
   lambda <- BoxCox.lambda(x)
   # Transform
   x_bc <- BoxCox(x,lambda))
   # Inverse transform
   a_hat <- InvBoxCox(x_bc, lambda)

The idea behind this transformation is to find a value of lambda that stabilises the variability of the time series.
  
**Bias adjustements**

When the forecast is back-transformend to the original domain, it will not be the mean of the predictive distribution, but its median. To make this the mean the bias adjustment should be applied. Look for the :code:`biasadj` parameter in the prediction functions.
  

Residual diagnostics
====================
  
Fitted values
-------------

Fitted values are the predictions of the training data point t based on all
the previous time points. The fitted values are not true forecasts if the 
model's parameters are estimated using all the available data, including those
that come after t.
  
Residuals
---------

Residuals typically are the difference between the fitted and the actual 
values of a time series

* Residuals should have:

  * Zero mean (they're biased otherwise)
  * no correlation (should be uncorrelated)


* Residuals nice to have:

  * Constant variance
  * Normal distribution
  
Prediction intervals are calculated based on the assumption that the residuals
are both uncorrelated and Normally distributed. If either of these conditions
does not hold, the prediction intervals may be invalid.   

Residual checking
-----------------

The following methods both check for autocorrelations in residuals. The null hypothesis is that the residuals are uncorrelated. A very small p-value therefore, implies that the resiuals still have some correlation in them.

.. code-block:: r

   # Automatic check of the residuals from the forecast package
   # It also sets automatically the number of lags to use in the 
   # Box test. 
   checkresiduals(naive(y))

   # Box test only. The lags need to be passed as arguments. Otherwise assumed to be 1.
   # Box-Pierce test 
   Box.test(res, lag=8)
   # Box-Ljung test
   Box.test(res, lag=8, type="Lj")


Autocorrelations
----------------

.. code-block:: r


   # Autocorrelation function
   q <- acf(x) 
   ggAcf(x) # ggplot variant

   # Partial autocorrelation
   q <- Pacf(x)
   ggPacf(x)

   # Show both Acf and Pacf in a single plot
   ggtsdisplay(x)


* In both cases the critical values are :math:`1.96/\sqrt{\textrm{length}(x)}`.

* The Partial autocorrelation shows the correlation between points :math:`n` and :math:`n-k`, after the effect of all the intermediate points (:math:`n-1, n-2, \ldots`) has been removed.


* The Box tests tend to be more accurate, because they test the autocorrelation coefficients at all lags collectively.
     

Checking for normal distribution
--------------------------------

.. code-block:: r


   # Checking for normal distribution
   qqnorm(res)



  
Evaluating forecast accuracy
============================
  
Metrics
-------

**Scale dependent errors**

On the same scale with the data, cannot be used to make comparisons across time series with different units

* Mean Absolute Error  :math:`\textrm{MAE = mean}(|e_t|)`

* Root Mean Square Error  :math:`\textrm{RMSE} = \sqrt{\textrm{mean}(|e_t^2|)}`

**Percentage Errors**

Based on a percentage between the error and the actual value :math:`y_t`

* Mean Absolute Percentage Error  :math:`\textrm{MAPE} = \textrm{mean}(|100 e_t / y_t|)`
* Symmetric MAPE :math:`\textrm{sMAPE} = \textrm{mean}(200|y_t - \hat{y}|/(y_t + \hat{y}_t))`


**Scaled Errors**

Useful when comparing forecasts across series with different units

* Mean Absolute Scaled Error  

Calculation
-----------

The forecast accuracy can be checked with the following, assuming that :code:`fc` is a :code:`forecast` class object.

.. code-block:: r

   # Accuracy metrics based on the testing data
   accuracy(fc)
   # Accuracy metrics between predictions and the test data
   accuracy(fc, test.data)

    
Another useful method is the *Time Series Cross Validation*, especially useful when the data points are limited and a train/test split is not possible without severely limiting the quantity of training data.

.. code-block:: r

    e <- tsCV(goog201, rwf, drift=TRUE, h=8)
    sqrt(mean(e^2, na.rm=TRUE))

    
A Forecasting Strategy
======================
* Plot the data and identify unusual observations; Understand patterns

   * use autoplot, ggseasonplot, ggsubseriesplot, gglagplot, ggAcf
   * Identify if there is any trend or any seasonality

* If necessary, use a Box-Cox transformation to stabilize the variance.

* Choose a model depending on whether there is a trend or a seasonality

* Fit the model and check the results using summary(fc)

* Check the residuals by plotting the ACF of the residuals and doing a portmanteau test

* Calculate forecasts and evaluate them with :code:`accuracy` or :code:`tsCV`.

Exponential Smoothing methods
*****************************

Simple exponential smoothing 
=============================

.. code-block:: r

    fc <- ses(data, h=4)

Holt's Linear Trend Method
==========================

.. code-block:: r

    # Undamped version
    fc <- holt(data, h=4)

    # Damped version
    fc <- holt(data, damped=TRUE, h=4)


Holt-Winter's seasonal method
=============================

This method captures seasonality. The seasonality component can be added or
multiplied to the level component. The first is preferred when the seasonal 
component is independent of the level, the second when the seasonal variations
are changing proportionally to the level of the series. The method also supports
a damped trend

.. code-block:: r

   fc <- hw(y, damped=TRUE, seasonal="multiplicative")

The ETS function
================

Most exponential smoothing models, can be classified according to the Error, 
Trend, Seasonal taxonomy, ETS for short. The possibilities for each component
are

============= ====================================== ==============
  Component                 Type                         Short
------------- -------------------------------------- --------------
Error           {Additive, Multiplicative}            {A, M}
Trend           {None, Additive, Additive damped}     {N, A, Ad}
Seasonal        {None, Additive, Multiplicative}      {N, A, M}
============= ====================================== ==============

The ETS models can be fitted using the :code:`ets` function

.. code-block:: r

   ets(y, model="ZZZ", damped=NULL, alpha=NULL, beta=NULL,
   gamma=NULL, phi=NULL, lambda=NULL, biasadj=FALSE,
   additive.only=FALSE, restrict=TRUE,
   allow.multiplicative.trend=FALSE)

y
   The time series to be forecast.
model
   A three-letter code indicating the model to be estimated using the ETS
   classifcation and notation. The possible inputs are “N” for none, “A” for
   additive, “M” for multiplicative, or “Z” for automatic selection. If any of the
   inputs is left as “Z”, then this component is selected according to the
   information criterion. The default value of  ZZZ  ensures that all components
   are selected using the information criterion.
damped
   If :code:`damped=TRUE`, then a damped trend will be used (either A or M). If
   :code:`damped=FALSE`, then a non-damped trend will used. If :code:`damped=NULL`
   (the default), then either a damped or a non-damped trend will be selected,
   depending on which model has the smallest value for the information
   criterion.
alpha, beta, gamma, phi
   The values of the smoothing parameters can be specified using these
   arguments. If they are set to  :code:`NULL` (the default setting for each of them),
   the parameters are estimated.
lambda
   Box-Cox transformation parameter. It will be ignored if :code:`lambda=NULL` (the
   default value). Otherwise, the time series will be transformed before the
   model is estimated. When lambda is not :code:`NULL`, additive.only is set to :code:`TRUE`.  
biasadj
   If :code:`TRUE` and lambda is not :code:`NULL`, then the back-transformed fitted values
   and forecasts will be bias-adjusted.
additive.only
   Only models with additive components will be considered if
   :code:`additive.only=TRUE`. Otherwise, all models will be considered.
restrict
   If  :code:`restrict=TRUE` (the default), the models that cause numerical difficulties
   are not considered in model selection.
allow.multiplicative.trend
   Set this argument to :code:`TRUE` to allow multiplicative trend models models to
   be considered.

**Working with ETS models**

coef()
   returns all fitted parameters.
accuracy()
   returns accuracy measures computed on the training data.
summary()
   prints some summary information about the fitted model.
autoplot()  and  plot()
   produce time plots of the components.
residuals()
   returns residuals from the estimated model.
fitted()
   returns one-step forecasts for the training data.
simulate()
   will simulate future sample paths from the fitted model.
forecast()
   computes point forecasts and prediction intervals


The forecast function
=====================

The forecast :code:`function` can be used to produce forecasts that pop out of the :code:`ets`
function. Its signature is

.. code-block:: r

   forecast(object, h=ifelse(object$m>1, 2*object$m, 10),
   level=c(80,95), fan=FALSE, simulate=FALSE, bootstrap=FALSE,
   npaths=5000, PI=TRUE, lambda=object$lambda, biasadj=NULL, ...)

object
   The object returned by the :code:`ets()` function.
h
   The forecast horizon — the number of periods to be forecast.
level
   The confidence level for the prediction intervals.
fan
   If  :code:`fan=TRUE`,  :code:`level=seq(50,99,by=1)`. This is suitable for fan plots.
simulate
   If :code:`simulate=TRUE`, prediction intervals are produced by simulation rather
   than using algebraic formulas. Simulation will also be used
   (even if :code:`simulate=FALSE`) where there are no algebraic formulas available for the
   particular model.
bootstrap
   If :code:`bootstrap=TRUE` and  :code:`simulate=TRUE`, then the simulated prediction
   intervals use re-sampled errors rather than normally distributed errors.
npaths
   The number of sample paths used in computing simulated prediction intervals.
PI
   If :code:`PI=TRUE`, then prediction intervals are produced; otherwise only point
   forecasts are calculated.
lambda
   The Box-Cox transformation parameter. This is ignored if :code:`lambda=NULL` .
   Otherwise, the forecasts are back-transformed via an inverse Box-Cox
   transformation.
biasadj
   If :code:`lambda` is not :code:`NULL`, the back-transformed forecasts (and prediction
   intervals) are bias-adjusted.



ARIMA models
************

+-------------------------------------------+
|         ARIMA(p, d, q) models             |
+===========================================+
| p = order of the autoregressive part      |
+-------------------------------------------+
| d = degree of first differencing involved;|  
+-------------------------------------------+
| q = order of the moving average part;     |
+-------------------------------------------+



+----------------------------------------------------+
| long term forecast  (c=constant, d=differencing)   |
+====================================================+
| c=0, d=0,  zero                                    |
+----------------------------------------------------+
| c=0, d=1,  non-zero constant                       |
+----------------------------------------------------+
| c=0, d=2,  straight line                           |
+----------------------------------------------------+
| c~=0 d=0,  mean of the data                        |
+----------------------------------------------------+
| c~=0, d=1, straight line                           |
+----------------------------------------------------+
| c~=0, d=2, quadratic trend                         |
+----------------------------------------------------+ 


Unit root tests (stationarity check)
====================================

The stationarity of a time series can be checked with the Kwiatkowski-Phillips-Schmidt-Shin (KPSS). The null hypothesis is that the data are stationary, and we look for evidence that the null hypothesis is false. Small p-values, suggest that the time series is non-stationary and differencing is required to make it stationary. 

The number of times differencing is required, can be found with the :code:`ndiffs` function.

If the signal seems to have a seasonal component, it can be checked with seasonal differencing, using the :code:`nsdiffs` function. If this is the case, it is advisable to remove the seasonal component first before taking first order differences. The existence of a seasonal component can be checked with the :code:`ggsubseriesplot`.

.. code-block:: r

   library(urca)
   # Carry out the KPSS test
   summary(ur.kpss(x))
   # Determine the number of times differencing is required to make it stationary.
   ndiffs(x)
   # Determine the number of times seasonal differencing is required 
   nsdiffs(x)




Autocorrelations
================

.. code-block:: r

   # Autocorrelation
   ggAcf(x)

   # Partial autocorrelation
   ggPacf(x)

   # Show both
   ggtsdisplay(x)

* In both cases the critical values are :math:`1.96/\sqrt{\textrm{length}(x)}`.
* The Partial autocorrelation shows the correlation between points :math:`n` and :math:`n-k`, after the effect of all the intermediate points (:math:`n-1, n-2, \ldots`) has been removed.
* An ARIMA(p, d, 0) (AR) model will the last significant spike in the PACF in
  :math:`p`, and a decaying ACF. 
* An ARIMA(0, d, q) (MA) model will have the last significant spike in the ACF
  in :math:`q` and a decaying PACF.
* Similar conclusions are harder to draw from general ARIMA(p, d, q) models.


Modelling procedure
===================

1. Plot the data and look for the following: 

   * Trend
   * Seasonality
   * Changes in variance
   * Anything unusual

2. If the variance changes with time, a Box-Cox transformation can be used to stabilise it.
3. If the data are non-stationary, take first differences of the data until the data are stationary.

   * Use the Unit Root test (:code:`ur.kpss`)
   * Find the number of differences with :code:`ndiffs`
   * Find the number of seasonal differences with :code:`nsdiffs`
   * Remove the seasonal differences first, if any.

4. Examine the ACF/PACF: Is an ARIMA(p, d, 0) or ARIMA(0, d, q) model appropriate?

   * Use the :code:`ggAcf`, :code:`ggPacf`, :code:`ggtsdisplay`,

5. Try your chosen model(s), and use the AICc to search for a better model.
6. Check the residuals from your chosen model by plotting the ACF of the residuals, and doing a portmanteau test of the residuals. If they do not look like white noise, try a modified model.

   * Use the :code:`checkresiduals`, :code:`Box.test` and autocorrelation plots.

7. Once the residuals look like white noise, calculate forecasts.

   * Evaluate with test data or with :code:`tsCV`.

Auto arima
==========



:code:`stepwise=FALSE` and :code:`approximation=FALSE` makes auto.arima work harder for finding the appropriate model!

Information Criteria should not be used for selecting between different orders of differencing (:math:`d`), because differencing changes the data on which the likelihood is computed. Some other approach has to be used to first identify :math:`d` and the information criteria can then be used for estimating :math:`p` and :math:`q`. The value of :math:`d` can be estimated using the KPSS stationarity test. 

Some times its not possible to find a model that passes all the residual tests. In these cases, the model that scores best at a test set is used.



The rpy2 library
################

The rpy2 library allows calling R from Python.

General 
********

Check the rpy2 version

.. code-block:: python

  import rpy2
  print(rpy2.__version__)

The following will print the 'situation' regarding the R and python versions in use, environment variables etc

.. code-block:: python

  import rpy2.situation
  for row in rpy2.situation.iter_info():
    print(row)

R's objects are exposed with :code:`robjects`

.. code-block:: python

  import rpy2.robjects as robjects

Rpy2 provides an R instance to work with 

.. code-block:: python

  from rpy2.robjects import r 

R packages can be imported as follows

.. code-block:: python

  from rpy2.robjects.packages import importr
  base = importr('base')

Working with the R instance
***************************

An R expression can be evaluated as follows

.. code-block:: python

   r('list(a=1:3, b=4:6)')

The result of the expression can be returned to python

.. code-block:: python

   l = r('list(a=1:3, b=4:6)')

:code:`l` is now a ListVector with 2 elements

Any object in R's workspace can be accessed from R's instance as follows

.. code-block:: python

   r('x <- list(a=1:3, b=4:6)') # Create the list x in R
   x = r['x'] # Get the list back into Python

A two-way interaction with R's global environment is through :code:`robjects.globalenv`

.. code-block:: python

   r('x <- list(a=1:3, b=4:6)')  # Create the list in R
   x = robjects.globalenv["x"]   # Get it in python
   robjects.globalenv["z"] = x   # Put it back in R under the name 'z'


Creating R objects in Python
****************************

R objects can be created in Python using the following classes

.. code-block:: python

   res = robjects.StrVector(['abc', 'def']) # String vector
   res = robjects.IntVector([1, 2, 3]) # Integer vector
   res = robjects.FloatVector([1.1, 2.2, 3.3]) # Integer vector

   # List vector (the equivalent of a dictionary)
   res = robjects.ListVector({'a':robjects.FloatVector([1.1, 2.2, 3.3]),
                              'b':robjects.IntVector([1, 2, 3])})


Calling R functions
*******************

An R function can be called with an R object in Python as simply as 

.. code-block:: python

   r.names(x)
   r['names'](x)
   # Or even
   names = r['names']
   names(x)

In the above, :code:`x` is an :code:`rpy2.robject`, which is used as an argument to an R function straight from python.

The same can be done with a package function. Note that packages don't support subscription

.. code-block:: python

   from rpy2.robjects.packages import importr
   base = importr('base')

   base.names(x)
   names = base.names
   names(x)

   # base['names'](x) would not work!!


Extracting an item from a list
******************************

Let

.. code-block:: python

   x = r('list(a=c(1,2,3,4))')

Rpy2 supports two extraction styles, an R-Style and a Python-style. 

**R-Style extraction**

The following will return a ListVector with 1 element (the sublist 'a').
This is equivalent of :code:`[` in R.

.. code-block:: python

   x.rx('a')
   x.rx(1) # 1-based

   # ListVector with 1 elements.
   # a	FloatVector with 4 elements.
   # 1.000000	2.000000	3.000000	4.000000

The following will return a FloatVector with 4 elements (the contents of the sublist 'a').
This is the equivalent of :code:`[[` or :code:`$` in R)

.. code-block:: python

   x.rx2('a')
   x.rx2(1)

   # FloatVector with 4 elements.
   # 1.000000	2.000000	3.000000	4.000000
   
**Python-Style extraction**

The following is equivalent to the rx2 example above, but it cannot accept named arguments and is also 0-based.

.. code-block:: python

   x[0]

   # FloatVector with 4 elements.
   # 1.000000	2.000000	3.000000	4.000000

The print function
******************

The :code:`print` function returns handy information on R objects in python

.. code-block:: python

   print(x)


