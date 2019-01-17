A tool for efficiently grabbing usable metadata and experiment data off GEO for use in R.

Getting experiment metadata and GSE accession numbers
-----------------------------------------------------

Metadata by experiment accession number can be returned for **all** GEO experiments or **by GPL platform code**

``` r
devtools::load_all("~/repos/geograbr")
require("XML")
```

``` r
## all datasets in GEO
datasets <- geograbr.retrieve.datasets()  ## about 4min
```

``` r
## all Illumina Infinium 27K DNA methylation datasets
datasets <- geograbr.retrieve.datasets(gpl = c("GPL13534")) ## 5sec
```

    > Thu Jan 17 14:02:17 2019 retrieving dataset UID 1 ...
    > Thu Jan 17 14:02:23 2019 retrieved 1192

``` r
str(datasets)
```

    > 'data.frame': 1192 obs. of  9 variables:
    >  $ title      : chr  "Epigenome analysis of depressed and control subjects." "DNA methylation analysis in familial adenomatous polyposis" "Genome-Wide DNA Methylation Profiling of Oesophageal Cancer-Associated Myofibroblasts" "Genome-Wide DNA Methylation Profiling of Gastric Cancer-Associated Myofibroblasts" ...
    >  $ description: chr  "Genome wide DNA methylation profiling of depressed (n=489) and control subjects (n=210)  that were recruited at the Max Planck "| __truncated__ "DNA methylation of 23 familial adenomatous polyposis tumors. Infinium HumanMethylation450 BeadChip was used to obtain DNA methy"| __truncated__ "The Illumina Infinium HumanMethylation450 BeadChip arrays were performed on a collection of primary oesophageal cancer-associat"| __truncated__ "The Illumina Infinium HumanMethylation450 BeadChip arrays were performed on a collection of primary gastric cancer-associated m"| __truncated__ ...
    >  $ organism   : chr  "Homo sapiens" "Homo sapiens" "Homo sapiens" "Homo sapiens" ...
    >  $ type       : chr  "Methylation profiling by genome tiling array" "Methylation profiling by array" "Methylation profiling by array" "Methylation profiling by array" ...
    >  $ platform   : chr  "GPL13534" "GPL13534" "GPL13534" "GPL13534" ...
    >  $ accession  : chr  "GSE125105" "GSE109507" "GSE97687" "GSE97686" ...
    >  $ id         : chr  "200125105" "200109507" "200097687" "200097686" ...
    >  $ project    : chr  "" "" "" "" ...
    >  $ samples    : int  699 23 10 9 55 60 329 22 148 138 ...

Download GEO series matix files for by GSE experiment numbers
-------------------------------------------------------------

``` r
## by individual gses
filenames <- geograbr.download.series.files(gses = "GSE94734")

## or for a vector of gses
filenames <- geograbr.download.series.files(gses = datasets$accession[1:6])
```

Grab sample-level data from downloaded series matrix files
----------------------------------------------------------

``` r
gse94734 <- geograbr.get.samples(filename = "GSE94734_series_matrix.txt.gz")

gses <- lapply(na.omit(filenames), geograbr.get.samples)
```

Grab experiment data from downloaded series matrix files
--------------------------------------------------------

Automatically saves data to `.rdata` format at user-defined path

``` r
test <- geograbr.get.data(filename = "GSE94734_series_matrix.txt.gz")
```