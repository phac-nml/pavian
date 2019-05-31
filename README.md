# Pavian 

Pavian is a interactive browser application for analyzing and visualization metagenomics classification results from classifiers such as 
Kraken, Centrifuge and MetaPhlAn. Pavian also provides an alignment viewer for validation of matches to a particular genome.

For more information look at the preprint at http://biorxiv.org/content/early/2016/10/31/084715.full.pdf+html . Please cite the preprint if you use Pavian in your research.

You can try out Pavian at https://fbreitwieser.shinyapps.io/pavian/.

## Galaxy Project Implementation

The Galaxy Branch modifies the traditional implementation of Pavian to include a modified version of R-Galaxy-Connector made specifically for [Pavian](https://github.com/justinband/r-galaxy-connector/tree/pavian)

This version of Pavian was made to be a Galaxy Interactive Envrionment inside of Galaxy Project. One of the key features is that it can access your datasets and collections from your current Galaxy history. This eliminates the need for downloading and reuploading your datasets and is much quicker at accessing collections than it would be otherwise.

## Installation and deployment

Pavian is an R package, and thus requires R to run. Look [here](http://a-little-book-of-r-for-bioinformatics.readthedocs.io/en/latest/src/installr.html) for how to install R. On Windows, you probably need to install [Rtools](cran.r-project.org/bin/windows/Rtools/). On Ubuntu, install `r-base-dev`. Once you started R, the following commands will install the package:

```r
if (!require(remotes)) { install.packages("remotes") }
remotes::install_github("justinband/pavian", ref='galaxy')
```

To run Pavian from R, type

```r
pavian::runApp(port=5000)
```

Pavian will then be available at <http://127.0.0.1:5000> in the web browser of you choice.

Alternatively, you can install and test Pavian with the following command:

```r
shiny::runGitHub("justinband/pavian", subdir = "inst/shinyapp")
```

## Installing Rsamtools

The alignment viewer uses `Rsamtools`. To install this package from Bioconductor, use the following commands

```r
source("https://bioconductor.org/biocLite.R")
biocLite("Rsamtools")
```

## Docker Image

As an alternative to installing Pavian in R, a Docker image can be used. The Docker image for pavian in galaxy is yet to be made available. However, the image for the original pavian is available at [florianbw/pavian](https://hub.docker.com/r/florianbw/pavian/). When you run this docker image, Pavian will start automatically on port 80, which you need to make available to the hosting machine. On the shell, you can pull the image and remap the Docker port to port 5000 with the following commands:

```sh
# Get the docker image
docker pull 'jband/pavian'

# Run the docker image
docker run --rm -p 5000:80 jband/pavian
```

## Screenshots

![image](https://cloud.githubusercontent.com/assets/516060/20188595/5c8b9808-a747-11e6-9235-296a2314659a.png)

[![Build Status](https://travis-ci.org/fbreitwieser/pavian.svg?branch=master)](https://travis-ci.org/fbreitwieser/pavian)

## Supported formats

pavian natively supports the Kraken and MetaPhlAn-style report formats. In extension, you can use Centrifuge results by running `centrifuge-kreport` on Centrifuge output files, and Kaiju results by running `kraken-report` on Kaiju output files (see issue #11)

## Too Many Files?

Originally, pavian's read_server_directory can only read directories with <= 100 files. I have increased this size to 300 to allow for larger collections of data to be passed through. Still, keep in mind a larger collection will load slower than a smaller collection.

## Error: Maximum upload size exceeded

The maximum upload size is defined by the option `shiny.maxRequestSize`. To increase it to 500 MB, for example, type the following before `pavian::runApp()`:

```r
options(shiny.maxRequestSize=500*1024^2)
```

If your BAM file contains the unaligned reads, you can decrease the file size before uploading by getting rid of non-aligned reads using samtools view -F4.
