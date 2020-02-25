###############################
Tips & Tricks & Troubleshooting
###############################

.. role:: python(code)
   :language: python


SExtractor
=============

* Make sure you are running the latest release from `the GitHub repository <https://github.com/astromatic/sextractor>`_.

* The warning *Significant inaccuracy likely to occur in projection* may occur if the input image header carries conflicting WCS header information. SCAMP will probably fail when run on the output SExtractor catalogues. Deleting any keywords of the form `PVx_y`, `CDELTAx`, `PROJPx` can help. See `https://www.astro.uni-bonn.de/theli/gui/faq.html <https://www.astro.uni-bonn.de/theli/gui/faq.html>`_, section *Coaddition*, for more information.

SCAMP
=============

* Make sure you are running the latest release from `the GitHub repository <https://github.com/astromatic/scamp>`_.

* Besides the official documentation, the University of Bonn also has a helpful page on `trouble-shooting SCAMP via the checkplots <https://www.astro.uni-bonn.de/theli/gui/astromphotom.html>`_.

* While they are not listed in the `pipeline_settings.ssos` file, you can pass `--ASTREF_CATALOG` and `--CROSSID_RADIUS` directly to the `ssos` script, which then passes the parameters on to SCAMP.

* If SCAMP gets stuck on the *Astrometric Matching* step, it may be because of problematic input image headers. Apparently, `SCAMP does not like zenithal/azimuthal polynomial (ZPN) projections <https://www.astromatic.net/forum/showthread.php?tid=319>`_. Changing the `CTYPEx` keywords to `RA---TAN` or `DEC--TAN` respectively (notice that both have to be 8 characters long) may help (this is automatically done by the pipeline). Also watch out for subsequent warnings in SExtractor runs, specifically *Significant inaccuracy likely to occur in projection*. Refer to the section above for possible solutions.

* When passing multi-extension FITS files to SCAMP, the auxilliary header files `.ahead` need to mirror this format as well. Each keyword has to be repeated on a new line for each header, and these additional headers have to be terminated by `END` cards. Again, this is automatically handled by the pipeline.


The `FLAGS_SCAMP` output parameter is a binary code (like the SExtractor flags) and refers to the following issues:

* 1 is for sources being rejected from the astrometric solution
* 16 is for sources being rejected from the photometric solution
* 32 is for sources without color indices
* 64 is for sources being rejected from the linear proper motion fit

Pipeline
=============

* In the `pipeline_settings.sso` file, paths containing `$HOME` will be expanded to the user home directory following :python:`os.path.expanduser('~')`.

* A quick sanity check of the SkyBoT matching can be done using TOPCAT, by comparing the proper motion of the SSOs as retrieved from the pipeline and as predicted by SkyBoT. If the matched SSOs are the ones recovered by the pipeline (and not a coincidental match), you expect ratios close to one.


* `OBSERVATORY_CODE`, `FOV_DIMENSIONS`, `REFERENCE_FILTER` are the only settings which are not checked for correct input upon the pipeline initialization.

* Asteroids can be associated to stars if they pass close by or temporarily become too faint. A good way to finde these erroneous detections is by looking for detections flagged by SCAMP with the value 64
