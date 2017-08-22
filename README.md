# R Client API for Web Time Series Classification Service 

**wtscs** is an R Client package for handling Web Time Series Classification Service (WTSCS) in the client side.

## Build the package:

Clone the project: git clone https://github.com/e-sensing/wtscs.git.

Open Rstudio, go to File - Open Project and pick the file wtscs.Rproj.

Install the required package install.packages("roxygen2").

Go to the Build tab in the upper-right panel and press the button Build & Reload. 

## Getting started

Installing and loading wtscs package

``` r
devtools::install_github("e-sensing/wtscs") # github repository name is wtsps
library(wtscs) # R package name is wtscs
```

A simple example of creating a WTSCS connection

``` r 
wtscs.server <- WTSCS(serverURL = "inst/extdata/wtscs/")
```

The result is an Object of Class WTSCS. 

``` r
wtscs.server
```

``` r
An object of class "WTSCS"
Slot "serverURL":
[1] "inst/extdata/wtscs/"

Slot "algorithms":
[1] "TWDTW"
```
It is possible to get the list of algorithms provided by a WTSCS service using a WTSCS object or simply a WTSCS server URL.

``` r
listAlgorithms(serverInfo = "inst/extdata/wtscs/")
```

``` r
## [1] "TWDTW"
```

We are also able to acquire any algorithm metadata with describeAlgorithm. This function returns an Algorithm class containing its name, input_parameters, output and a description using a WTSCS object already created or directly a WTSCS server URL. 

```r
describeAlgorithm(serverInfo = wtscs.server, name = "TWDTW")
```
``` r
An object of class "Algorithm"
Slot "name":
[1] "TWDTW"

Slot "input_parameters":
         patterns       dist.method             alpha              beta             theta          interval 
"twdtwTimeSeries"       "character"          "double"          "double"          "double"       "character" 
             span              keep           overlap        start_date          end_date 
        "integer"         "boolean"          "double"       "character"       "character" 

Slot "output":
     from        to     label  distance 
"integer" "integer" "integer"  "double" 

Slot "description":
[1] "Time-Weighted Dynamic Time Warping (TWDTW) method for land use and land cover mapping using satellite image time series."
```

Using input_parameters we can run any process with that algorithm as many parameters as possible.

```r
run_process <- runProcess(serverInfo = "inst/extdata/wtscs/", name = "TWDTW", patterns = "X")
```
```r
run_process
An object of class "Process"
Slot "uuid":
[1] 1

Slot "status":
[1] "Scheduled"

Slot "command":
[1] "name=TWDTW,patterns=X"
```

After that, WTSCS generates an identifier for this process so the user can see its status if necessary.

```r
status.proc <- statusProcess(serverInfo = "inst/extdata/wtscs/", processInfo = 1)
```
```r
status.proc
An object of class "Process"
Slot "uuid":
[1] 1

Slot "status":
[1] "In Progress"

Slot "command":
[1] "name=TWDTW,patterns=X"
```

Or even cancel the process.

```r
cancel.proc <- cancelProcess(serverInfo = "inst/extdata/wtscs/", processInfo = 1)
```
```r
cancel.proc
An object of class "Process"
Slot "uuid":
[1] 1

Slot "status":
[1] "Cancelled"

Slot "command":
[1] "name=TWDTW,patterns=X"
```

## Reporting Bugs

Any problem should be reported to esensing-developers@dpi.inpe.br.