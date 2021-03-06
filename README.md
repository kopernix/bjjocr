bjjocr
======

Script to automatically perform zonal OCR on a PDF and rename the PDF according to the results. It uses tesseract, imagemagick and pdftk.

This script scratches a specific itch our HR department has, namely to process thousands of uniform PDFs twice a month. As such it is geared towards many same-looking PDFs that come in bunches of several hundreds pages per PDF. We need each page as a single sheet identifiable by an ID on the page. So we first burst the PDF into single page PDFs and then run OCR over a zone on each PDF and put the result into a temporary file. We read the temporary file and rename the single page PDF accordingly.

In principle we can do this with as many zones as we like but we must initiate a new tesseract run for every zone.

# Quickstart:
# 1) Install required packages

We run a Debian 7 installation:

 $apt-get install tesseract-ocr tesseract-ocr-[yourlanguage] imagemagick pdftk

# 2) Convert your source file to a tiff-image. 

The tiff must fullfill the following requirement
* 300dpi
* 8 bit colordepth

I recommend stripping and trimming, too. Use ImageMagick's convert to create the tiff file:

 $convert -depth 8 -density 300 -strip -trim input.pdf output.tiff

# 3) Use zonal OCR with tesseract. 

Tesseract can perform zonal OCR if one or more appropriate zone files are provided. Zonefiles must use the extension .uzn and the same name as the input file, e.g input file = input.tiff then the uzn file must be named input.uzn

The .uzn file format is:
 
 x-coordinate y-coordinate width height identifier

* The x- and y-coordinates define the top left corner of a rectangle. 
* Width and height define the dimensions of the rectangle. 
* The identifier is unused and currently only helps the user in remembering the defined zone.
* All parameters must be one space apart.
* You can define several zones in one file, but only the first one is used.

 $tesseract input.tiff - -l <yourlanguage> -psm 4 

Instead of stdout you can use a freely chosen filename. The file will have the extension .txt.

#Future plans

If we can't get the commercial solution working to our demands, we will implement

1) parallel processing of many PDFs.

2) a folder watchdog ([id]notify) to run the script whenever a PDF is dropped into the watched folder.

3) Learn github's markup.

