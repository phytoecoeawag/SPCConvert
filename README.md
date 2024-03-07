## == SPCConvert - Raw ROIs to rendered mosaics and stats with a web application == #

this version is the latest, currently implemented for automatic publication of images and extraction of features as in https://aquascope.ch/latest-data/
this version is implemented for python 3

the text description below comes from the original script, published at https://github.com/tooploox/SPCConvert

## === Overview === #
SPCConvert is a collection of python scripts to convert raw region of interest (ROI) images from a real-time plankton camera such as the SPC into statistics and image mosaics.
Version: 1.10
Authors:
Paul L. D. Roberts, Eric Orenstein
additional object features: Filip Mróz
Quickstart
The fastest way to get your system setup is to install Anaconda and then add the opencv3 and pystache python modules through conda:

Install Anaconda for python 2.7 from https://anaconda.org/
Open a terminal and install the required python modules:
$ conda install -c menpo opencv3
$ conda install -c conda-forge pystache
$ conda install -c conda-forge pandas
Change to the SPCconvert directory and run it
$ python spcconvert.py [path_to_image_dir]
Requirements
python 2.7, numpy, scipy, skimage, opencv3, pystache, pandas
Tested on mac, and windows
The requirements are also listed in requirements.txt file.

settings.xml
Some of the basic processing steps can be configured by editing settings.xml.

Detailed Description
SPCconvert takes as input a directory or directory of directories of images and builds image mosaics and stats from these images. It is designed to take as input images from real-time ROI detection systems such as the Scripps Plankton Camera (http://spc.ucsd.edu) and build a web app that is similar to the image browsers from spc.ucsd.edu

The code collects all of the images in the directory, loads them into RAM and then splits the work of processing them over multiple processes up to NCPU-1. The processing consists of:

color conversion
edge-detection and segmentation
morphological feature extraction
foreground masking
inverse filtering of masked foreground
The results are then saved into a new directory structure and a web app is build from the data using pystache (mustache) templates and some javascript and html files stored in the app directory.

Additionally spreadsheet (features.tsv) is created with features of every detected object. These are vastly more detailed that those available in browser - especially intensity measurements can be found useful in further analysis of the observed entities.

Morphological and colour features in spreadsheet
For every object in the image a number of properties is calculated (in features.tsv) which can be used for analysis (e.g. organism classification). The shape related properties are:

aspect_ratio, eccentricity - describes how much elongated is the shape
maj_axis_len, min_axis_len - describes the length of the organism in two directions - major and minor [mm]
orientation - orientation of the organism in the image in degrees
solidity - proportion of the pixels in shape to the pixels in the convex hull - for a fish it is almost one and for a starfish it is very low as its convex hull is much bigger that starfish itself.
estimated_volume - approximation of the volume of the shape in [mm^3]
area - area of the shape in real world metric [mm^2]
The colour properties are calculated using images rescaled to fit 0-255. The properties are calculated separately for grayscale and colour components (R,G,B). Intensity is the value of the pixels in the chosen channel:

intensity_[color]_mean_intensity - average intensity of the organism in the image
intensity_[color]_perc_25_intensity - first quartile (25%) of intensity of the organism in the image
intensity_[color]_median_intensity - second quartile (50%) of intensity of the organism in the image
intensity_[color]_perc_75_intensity - third quartile (75%) of intensity of the organism in the image
intensity_[color]_std_intensity - standard deviation in the intensity pixels - how much variability is in the intensity of the organism in the image
intensity_[color]_mass_displace - describes the distance between the center of intensity and center of shape - if there is a bright spot in one part of the organism then it is the distance from center to that spot.
intensity_[color]_mass_displace_in_images - distance is scaled by the size of the input image
intensity_[color]_mass_displace_in_minors - distance is scaled by the minor axis length of the organism in the image
intensity_[color]_moment_hu_1 (1 to 7) - these are numbers designed to describe the shape and the distribution of intensity of the image. They are excepted to be scale, translation and rotation invariant so can be used in classification of the organisms.
Configuring for a Specific Camera
There are a few settings that need to be adjusted to use with a different camera. The primary ones are:

BayerPattern: This needs to match your camera, for example BG is used for the SPC, SPCP cameras and RG is used for the SPC-BIG, UW, USC cameras
PixelSize: This is pixel size of pixels in object space in um/pixel of the given camera/lens combination, it is converted to mm/pixel in the code
SPC: 7.38 um/pixel (Sony ICX814, 3.69 um pixels with 0.5x objective)
SPCP: 0.62 um/pixel (Sony ICX834, 3.1 um pixels with 5x objective)
SPC2: 7.38 um/pixel (Sony ICX814, 3.69 um pixels with 0.5x objective)
SPCP2: 0.738 um/pixel (Sony ICX814, 3.69 um pixels with 5x objective)
SPC-PWSSC, NOAA: 22.6 um/pixel (Sony ICX834, 3.1 um pixels with 0.137x objective)
SPC-BIG, UW, USC: 25.18 um/pixel (Sony IMX253, 3.45 um pixels with 0.137x objective)
Contribution guidelines
Help is needed to improve this code. Specific areas are:
Improved control of processing and app building parameters via settings.xml
Managementr of cases with more images then can fit in RAM
Saving morphological and color data to a deparate csv file
Handling bad images, images that are too large or small
Contact Paul Roberts (plroberts@ucsd.edu) for information about contributing code, bugs, and all things SPC related.
About
No description, website, or topics provided.
Resources
 Readme
License
 MIT license
 Activity
 Custom properties
Stars
 2 stars
Watchers
 6 watching
Forks
 1 fork
Report repository
Releases
No releases published
Packages
No packages published
Languages
Python
70.9%
 
HTML
13.8%
 
JavaScript
12.2%
 
CSS
3.1%
Footer
