#
R
#

Change directory
****************

:code:`setwd()`

:code:`getwd()`


Execute system commands
***********************

:code:`system`

Read/write binary files
***********************

:code:`fid = file('filename.bin','rb')`

:code:`seek(fid, where=0, origin=c("start", "current", "end"))`
    * positions the file pointer
:code:`seek(fid)`
    * returns the position of the pointer

:code:`a = readBin(fid,double(),n)`
    * where n is the number of elements (they can be read from the size of file)

:code:`writeBin(X,fid,double())`

:code:`close(fid)`



Matrix
******
:code:`X = matrix(X, nrow=xx, ncol=xx, byrow=F)`

Managing variables
******************
:code:`ls.str()`: gives some more detail than ls()

:code:`rm(list=ls())`:  removes everything from the work space

Dataframes
**********
:code:`summary(variable)`
    * gives a summary of the variable

:code:`names(x)`
    * gives the names of the dataframe

Contigency table
****************
:code:`table(x,y)`
    * Build a contigency table using x,y

Save variables 
***************
:code:`save(x, y, file = "xy.RData")`

Packages
********
:code:`require(package.name)`
    * loads package
:code:`install.package(package.name)`
    * installs the package
:code:`installed.packages()`
    * lists all the installed packages
:code:`search()`
    * lists all the loaded packages

:code:`data()`
    * lists the datasets in the loaded packages
:code:`data(data.in.a.package)`
    * loads the data set in the global workspace



