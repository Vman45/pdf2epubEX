# pdf2epubEX

This Bash script uses the *pdf2htmlEX* tool to convert a PDF file to an ePub file.

The result is a *fixed layout* ePub: the layout is perfectly retained and all the fonts are embedded.

The *pdf2htmlEX* tool converts a PDF file into HTML5 (with CSS, JS, fonts, and bitmap and/or vector images). This means that the pages are not just converted into images as a lot of converters are doing.

## Using the Bash script

### Usage

To convert myfile.pdf to myfile.epub, run the following command in the directory where the PDF file is located:

```
./pdf2epubEX.sh myfile.pdf
```

Result will be: myfile.epub

### Prerequisites

- Download the Bash script: [pdf2epubEX.sh](https://raw.githubusercontent.com/dodeeric/pdf2epubEX/master/pdf2epubEX.sh).
- Install *pdf2htmlEX* and some other utilities: poppler-utils, bc, zip and file. If you are using Linux Debian or a Debian based Linux distribution (Ubuntu, Mint, etc.):

```
apt-get install ./pdf2htmlEX-0.18.8.rc1-master-20200630-Ubuntu-focal-x86_64.deb
apt-get install poppler-utils bc zip file
```

The Debian package (.deb) is available in this repository.

If you install *Git*, you can also just do:

```
git clone https://github.com/dodeeric/pdf2epubEX.git
```

## Using the Docker image

A Docker image is vailable on [my DockerHub repository](https://hub.docker.com/r/dodeeric/pdf2epubex).

### Usage

To convert myfile.pdf to myfile.epub, run the following command in the directory where the PDF file is located:

```
docker run -ti --rm -v `pwd`:/pdf dodeeric/pdf2epubex pdf2epubEX.sh myfile.pdf
```

The result will be: myfile.epub

You can also use *pdf2htmlEX* with this same Docker image:

To convert myfile.pdf to myfile.html, run the following command in the directory where the PDF file is located:

```
docker run -ti --rm -v `pwd`:/pdf dodeeric/pdf2epubex pdf2htmlEX myfile.pdf
```

The result will be: myfile.html

*pdf2htmlEX* has a lot of parameters. To see them:

```
docker run -ti --rm -v `pwd`:/pdf dodeeric/pdf2epubex pdf2htmlEX --help
```

### Prerequisites

You need to install Docker which is available for all computer OS: Windows, MacOS, Linux and Unix. See [here](https://docs.docker.com/engine/install).

## Parameters

Once you launch *pdf2epubEX.sh*, some information will be displayed like the book/PDF width and height (in inches and cm), then some questions will be asked like:

- Format of the images in the epub (png, jpg or svg) [default: jpg]
- Resolution of the images in the epub in dpi (e.g.: 150 or 300) [default: 150]
- Title, Author, Publisher, Year, Language: (e.g.: fr), ISBN number, Subject (e.g.: history)

If you want, you can hit `ENTER` to all the questions.

Caution:
- if you chose png or jpg (bitmap formats), the vector images will be converted in bitmap images (rasterized)
- if you chose svg (vector and bitmap format), the vector images will remain in vector format, but: a) you cannot chose the resolution of the bitmap images (it is the one from the PDF); b) the bitmap images will be included in the svg files; c) this format is not always correctly rendered by eBook readers; d) the generated epub file is not always passing the epub check.

The ePub cover image will be made from the first page of the PDF file (png format).

## Examples

In the examples below, the HTML version is one big file including everything (all the pages with HTML5, CSS, JS, fonts and images; fonts and images are coded in Base64). *pdf2htmlEX* can also put all that content in different files (.html, .css, .js, .woff, .png, .jpg); that's in fact what basicaly the *pdf2epubEX.sh* script does before wripping all the files in one ePub container file (.epub). Sometime, ePub is referred as "website in a box".

For eBooks with a lot of bitmap images, it is better to chose JPG (compression with loss) to not have a file too big. For eBooks with mainly vector images, it is better to chose PNG (lossless compression).

**PNG or JPG format** (bitmap images only; vector images are converted to bitmap images):

- **Install your own OpenStack Cloud**, Eric Dodémont, 2012, 49 pp. (150 dpi / PNG): [PDF](https://dodeeric-web.s3.eu-central-1.amazonaws.com/Install-your-own-OpenStack-Cloud-Eric-Dodemont.pdf) | [HTML](https://dodeeric-web.s3.eu-central-1.amazonaws.com/Install-your-own-OpenStack-Cloud-Eric-Dodemont.html) | [ePub](https://dodeeric-web.s3.eu-central-1.amazonaws.com/Install-your-own-OpenStack-Cloud-Eric-Dodemont.epub)
- **La dynastie belge en images (Preview)**, Eric Dodémont, 2015, 248 pp. (150 dpi / JPG): [PDF](https://dodeeric-web.s3.eu-central-1.amazonaws.com/La-dynastie-belge-en-images-Preview-Eric-Dodemont.pdf) | [HTML](https://dodeeric-web.s3.eu-central-1.amazonaws.com/La-dynastie-belge-en-images-Preview-Eric-Dodemont.html) | [ePub](https://dodeeric-web.s3.eu-central-1.amazonaws.com/La-dynastie-belge-en-images-Preview-Eric-Dodemont.epub)
- **CEB 2015 - Solides et Figures**, 2015, 24 pp. (150 dpi / PNG): [PDF](https://dodeeric-web.s3.eu-central-1.amazonaws.com/CEB-2015-Solides-et-Figures.pdf) | [HTML](https://dodeeric-web.s3.eu-central-1.amazonaws.com/CEB-2015-Solides-et-Figures.html) | [ePub](https://dodeeric-web.s3.eu-central-1.amazonaws.com/CEB-2015-Solides-et-Figures.epub)

**SVG format** (bitmap and vector images):

Caution: a) No one of the three following files pass the epubcheck validation using EPUB version 3.2 rules ; b) PDF with a lot of bitmap images can become voluminous because these bitmap images are coded in Base64 in the SVG files.

- **Install your own OpenStack Cloud** (SVG): [ePub](https://dodeeric-web.s3.eu-central-1.amazonaws.com/Install-your-own-OpenStack-Cloud-Eric-Dodemont-300dpi-svg.epub) (Mainly with vector images, file size: 1.7 MB) 
- **La dynastie belge en images (Preview)** (SVG): [ePub](https://dodeeric-web.s3.eu-central-1.amazonaws.com/La-dynastie-belge-en-images-Preview-Eric-Dodemont-300dpi-svg.epub) (With a lot of bitmap images, file size: 528 MB)
- **CEB 2015 - Solides et Figures** (SVG): [ePub](https://dodeeric-web.s3.eu-central-1.amazonaws.com/CEB-2015-Solides-et-Figures-300dpi-svg.epub) (Mainly with vector images, file size: 1.0 MB)

A vector image can be as simple as a line, a rectangle, a table frame, a colored background, etc.

## Additional information

### Book

The script is based on the method described in my book published in 2014: *Fixed Layout ePub: A Practical Guide to Publish eBooks from PDF Files*. It is available on [Amazon](https://www.amazon.fr/dp/1502809508) and on [Googgle Play Books](https://play.google.com/store/books/details?id=LRQ-BQAAQBAJ).

### ePub Fix Layout

To read a fix layout ePub, the best device is a tablet (Android or iOS/iPad). A smartphone is not adapted most of the time because of the too small screen size.

A lot of ePub reader apps exist (to read reflowable text ePub and fixed layout ePub): Google Play Books, BookShelf, PocketBook, Apple Books (only on iOS; formely known as Apple iBooks), etc. 

Amazon Kindle does not support the standard ePub format (they have their own format which is based on the ePub format).

To use Google Play Books, you have to go to **Settings**, then set **Enable uploading (from downloads, mail or other apps)**. The uploaded eBooks (PDF or ePub) will be available on all devices using the same Google account. You can also upload eBooks from the [Google Play Books web interface](https://play.google.com/books) (see the **Upload files** button on the top right corner).
 
More about fixed layout (FXL) ePub version 3 specifications (IDPF / W3C): [Fixed Layouts (EPUB Content Documents 3.2)](https://www.w3.org/publishing/epub/epub-contentdocs.html#sec-fixed-layouts) and [Fixed-Layout Properties (EPUB Packages 3.2)](https://www.w3.org/publishing/epub/epub-packages.html#sec-package-metadata-fxl).

### Other Git Repositories

Repositories for *pdf2htmlEX*: the [original one](https://github.com/coolwanglu/pdf2htmlEX) and the [new one](https://github.com/pdf2htmlEX/pdf2htmlEX) (with updated .deb packages).

This script is based on the Bash scripts written by Robert Clayton (RNCTX) and available in [his Git repository](https://github.com/RNCTX/PDF2HTMLEX-EPUB3FIXED).
