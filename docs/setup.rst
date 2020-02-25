###############
Getting Started
###############

.. role:: bash(code)
   :language: bash


.. role:: python(code)
   :language: python

Installing the Pipeline
=======================

The ``ssos`` pipeline is available from the Python Package Index via [#]_

.. code-block:: bash

    $ pip install ssos

Alternatively, you can clone the `GitHub Repository <https://github.com/maxmahlke/ssos>`_ or download the `zip file <https://github.com/maxmahlke/ssos/archive/master.zip>`_.

If not installing with `pip`, the package dependencies can be installed by
running the following line within the package directory

.. code-block:: bash

    $ pip install -r requirements.txt

or by using the setup script:

.. code-block:: bash

    $ [sudo] python setup.py install

Make sure that the astrOmatic binaries :bash:`sex`, :bash:`scamp`, and :bash:`swarp` are in your :bash:`PATH` shell variable. Try it with e.g.

.. code-block:: bash

    $ sex --version
    SExtractor version 2.25.0
    $ scamp --version
    SCAMP version 2.7.8
    $ swarp --version
    SWarp version 2.38.1

After installing, you can verify that the install worked by entering ``ssos`` from anywhere in your system. It should give you the version and some basic instructions.


Pipeline Setting Files
======================

The default ``default.ssos`` file can be `found here <https://github.com/maxmahlke/ssos/blob/master/ssos/default.ssos>`_. It is a plain ASCII, designed very similar to the configuration files of SExtractor and SCAMP in order to make the user feel right at home. Short descriptions and default values of the parameters are below, for more detailed descriptions refer to the `Pipeline <pipeline.html>`_ page.

.. note::
    The default configuration file ``default.ssos`` and the auxiliary SExtractor, SCAMP, and SWARP setup files are included in the ``python`` package and can be copied to the current working directory using the :bash:`ssos -d` or :bash:`ssos --default` syntax.

.. _Guide to SExtractor: http://astroa.physics.metu.edu.tr/MANUALS/sextractor/Guide2source_extractor.pdf

.. _IAU Observatory Code: http://vo.imcce.fr/webservices/data/displayIAUObsCodes.php

.. _SkyBoT: http://vo.imcce.fr/webservices/skybot/?conesearch


.. table::
    :align: center

    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | Parameter             | Values  | Examples                        |Description                                                                |
    +=======================+=========+=================================+===========================================================================+
    | `SCI_EXTENSION`       | mixed   | 1 |  2 | 1,2 | all              | Index of science extension of FITS images. For details, see               |
    |                       |         |                                 | :ref:`sextractor_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `WEIGHT_IMAGES`       | mixed   | False | /tmp/weights            | Absolute path to weight images for SExtractor run. [#]_ If False,         |
    |                       |         |                                 | SExtractor runs with settings according to ``ssos.sex`` file.             |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `DATE-OBS`            | string  | DATE-OBS / DATE                 | FITS header keyword for observation date in ISOT or MJD format            |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FILTER`              | string  | FILTER                          | FITS header keyword for observation filter/band                           |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `EXPTIME`             | string  | EXPTIME / TEXP / EXP            | FITS header keyword for exposure time in seconds                          |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `SEX_CONFIG`          | string  | semp/sso.sex                    | SExtractor configuration file for source detection in the survey images.  |
    |                       |         |                                 | For details, see :ref:`sextractor_section`.                               |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `SEX_PARAMS`          | string  | semp/sso.param                  | SExtractor output parameter for source detection in the survey images.    |
    |                       |         |                                 | For details, see :ref:`sextractor_section`.                               |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `SEX_FILTER`          | string  |semp/gauss_2.5_5x5 .conv         | SExtractor convolution filter file for source detection in the survey     |
    |                       |         |                                 | images. For details, see :ref:`sextractor_section` and the                |
    |                       |         |                                 | `Guide to SExtractor`_.                                                   |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `SEX_NNW`             | string  | semp/sso.nnw                    | SExtractor neural network for galaxy-star differentiation. For details,   |
    |                       |         |                                 | see :ref:`sextractor_section` and the `Guide to SExtractor`_.             |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `SCAMP_CONFIG`        | string  | semp/sso.scamp                  | SCAMP configuration file to link source detections at different epochs,   |
    |                       |         |                                 | see :ref:`scamp_section`.                                                 |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `REMOVE_REF_SOURCES`  | bool    | True | False                    | Remove source detections close to reference catalogue sources,            |
    |                       |         |                                 | see :ref:`scamp_section`.                                                 |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `SWARP_CONFIG`        | string  | semp/sso.swarp                  | SWARP configuration file for creation of cutout images of SSO candidates, |
    |                       |         |                                 | see :ref:`optional`.                                                      |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FILTER_DETEC`        | bool    | True | False                    | Turn filter based on number of detections on or off.                      |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `DETECTIONS`          | integer |  1,2 |  1,2,3,4 | 1,5           | Sources with this number of detections are rejected.                      |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FILTER_PM`           | bool    |   True | False                  | Turn filter based on proper motion values on or off.                      |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `PM_LOW`              | float   |     0.                          | Lower limit on proper motion of sources. See :ref:`filter_section`.       |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `PM_UP`               | float   |     200.                        | Upper limit on proper motion of sources. See :ref:`filter_section`.       |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `PM_SNR`              | float   |      20.                        | Lower limit on signal-to-noise ratio of proper motion of sources.         |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FILTER_PIXEL`        | bool    |   True | False                  | Turn filter based on pixel positions on or off. See :ref:`filter_section`.|
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `DELTA_PIXEL`         | float   |      2.                         | Minimum number of pixel the centre position of the source has to shift by |
    |                       |         |                                 | over all exposures in X and Y. See :ref:`filter_section`.                 |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FILTER_MOTION`       | bool    |    True | False                 | Turn filter based on linearity of motion on or off.                       |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `IDENTIFY_OUTLIER`    | bool    |    True | False                 | Identify outliers in epoch-space and treat their motion separately.       |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `OUTLIER_THRESHOLD`   | float   |     2.                          | Threshold in Median Absolute Deviations for identification of outlier.    |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `R_SQU_M`             | float   |     0.95                        | Lower limit of R-Squared goodness-of-fit parameter for linear motion fit. |
    |                       |         |                                 | Must be between 0 and 1. See :ref:`filter_section`.                       |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FILTER_TRAIL`        | bool    |      True | False               | Turn filter based on constant trail parameters on or off.                 |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `RATIO`               | float   |      0.25                       | Lower limit on the ratio of the error on the weighted mean to the standard|
    |                       |         |                                 | deviation of the source ellipse parameters. See :ref:`filter_section`     |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    |`FILTER_BRIGHT_SOURCES`| bool    |      True | False               | Turn filter based on source distance to bright sources on or off.         |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `DISTANCE`            | float   |        300.                     | Minimum distance of source to bright star in star catalogue in arcsecond. |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `MAG_LIMITS`          | float   |        -99,99                   | Minimum and maximum magnitudes of bright sources in the catalogue.        |
    |                       |         |                                 | See :ref:`filter_section`.                                                |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `BRIGHT_SOURCES_CAT`  | string  | REFCAT | path/to/cat            | Use SCAMP reference catalogue or provide path to one,                     |
    |                       |         |                                 | e.g. `HYG <http://www.astronexus.com/hyg>`_. See :ref:`filter_section`    |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `CROSSMATCH_SKYBOT`   | bool    |     True | False                | Turn cross-matching with SkyBoT database on or off. See :ref:`optional`.  |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `CROSSMATCH_RADIUS`   | float   |        10.                      | Upper limit of distance between source candidate and SkyBoT source to     |
    |                       |         |                                 | be considered a match, in arcsecond. See :ref:`optional`.                 |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `OBSERVATORY_CODE`    | string  |        500                      | `IAU Observatory Code`_                                                   |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FOV_DIMENSIONS`      | string  |       0x0                       | Dimensions of exposure field-of-view in degrees, see `SkyBoT`_.           |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `EXTRACT_CUTOUTS`     | bool    |     True | False                | Turn cutout extraction with SWARP on or off. See :ref:`optional`.         |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `CUTOUT_SIZE`         | integer |        256                      | Size of cutouts in pixel, each dimension, see :ref:`optional`.            |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `FIXED_APER_MAGS`     | bool    |    True | False                 | Compute fixed aperture magnitudes for colours. See :ref:`optional`.       |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `REFERENCE_FILTER`    | string  |         gSDSS,uSDSS             | Filter to use as reference in SExtractor dual-image mode runs. Value has  |
    |                       |         |                                 | to correspond to `FILTER` keyword in FITS header. See :ref:`optional`.    |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+
    | `CHECKPLOTS`          | string  |SKYBOT_RESIUDALS,SKYBOT_PM,False | Checkplots to create. False for no checkplots, otherwise a comma-separated|
    |                       |         |                                 | list of possible checkplots, given in :ref:`optional`.                    |
    +-----------------------+---------+---------------------------------+---------------------------------------------------------------------------+

The configuration file can be formatted with tabs and spaces. Comments are marked with `#`. Lines beginning with # or newline characters are ignored.

.. note:: The pipeline script first checks if the `-c` flag is pointing to a configuration file. If not, it looks for a file called `default.ssos` in the current working directory. If no file is found, the hard-coded default values are used. Any parameter can be overwritten temporarily by using the appropriate flag, see :ref:`Command-Line API <Command-Line API>`.


Step-by-step workflow
=====================

It is unlikely that the pipeline will give you the optimum result (clean and complete sample of SSOs) right out-of-the-box. Before running ``ssos`` on thousands of images from an observation campaign, it is helpful to pick test images taken close to the ecliptic to find the right settings. Outlined here is a typical workflow to set-up the pipeline for batch processing of images. The order of these steps can be varied in some cases. Further help can be found on the `Tips & Tricks & Troubleshooting <misc.html>`_ page.

1. Create a directory to contain the configuration files. In the directory, run

.. code-block:: bash

    $ ssos --default

to copy the default configurations files into this directory. Adjust the `SEX_CONFIG`, `SEX_FILTER`, `SEX_PARAMS`, `SEX_NNW`, `SCAMP_CONFIG`, `SWARP_CONFIG` parameters in the ``ssos`` config file to point to the just created files.

2. Open some images in `DS9` and use the `Analysis` functionality to overplot known SkyBoT SSOs in the FoV. Make note of how many known SSOs are visible in how many images - these numbers will be a goal to aim for in the following fine-tuning steps

3. Adjust the image specific keywords in the ``ssos`` config file: `RA`, `DEC`, `OBJECT`, `DATE-OBS`, `FILTER`, `EXPTIME`. Insert the corresponding header keyword names. You may have to create or adjust this keywords for the pipeline using e.g. `wcstools`

4. Run the pipeline in default settings. Use `topcat` to inspect the output catalogues and find out why known SSOs were missed. You can load all catalogues into topcat and overplot them in RA-Dec space.

5. If an SSO is not in the full catalogue created by SCAMP, you have to adjust the SExtractor settings. To quickly test different settings, you can run the pipeline again using the debugging flag `-l DEBUG` and copy the necessary SExtractor command that is printed to the console. Using `Aladin`, you can overplot the SExtractor catalogues and the images and see why source detections are missing or were rejected.

6. If an SSO is in the full catalogue but not in the final sample, SCAMP may have mis-associated source detections. Open the full catalogue and click through the SSO detections in the graphical display in RA-Dec space. Do they share the same `SOURCE_NUMBER`? You may have to adjust the `CROSSMATCH_RADIUS` parameter.

7. If an SSO is correctly linked up by SCAMP, it is removed by the filter pipeline. Adjust the settings in the ``ssos`` config file.

8. Inspect the checkplots and output candidates: After running ``ssos`` with `EXTRACT_CUTOUTS` set to `True`, you can quickly inspect the output candidates sample by running

.. code-block:: bash

    $ ssos --inspect path/to/output/folder


where the output folder is the directory which contains the `cats` and `cutouts` directories. You are then shown videos of the recovered sources one after the other and can quickly classify them using the arrow keys: "left arrow" for artifact, "right arrow" for asteroid, "up arrow" for unknown/unclear. These classifications are saved in the output `csv` database. If a candidate was matched to a SkyBoT source, its designation is printed in the upper left part of the animation.

9. Once you are confident about the recovered candidates, the accuracy of the
   positions (check the `SKYBOT_RESIDUALS` checkplot), and included calibrated
   magnitudes as `MAG_CALIB` column to the database, you can run 

.. code-block:: bash

    $ ssos --mpc path/to/output/csv

on the pipeline CSV output file to convert it to the MPC submission format.

Apart from the ``default.ssos`` parameters listed above, you likely have to adjust the following files and parameters before running it the first time, mostly by setting them to the appropriate FITS header keywords of your images:


``ssos.sex``

    - `SATUR_KEY`

    - `GAIN_KEY`

    - `SEEING_FWHM`

    - `MAG_ZEROPOINT`


``semp/ssos.scamp``

    - `ASTRINSTRU_KEY`

    - `ASTRACCURACY_KEY`

    - `PHOTINSTRU_KEY`

    - `MAGZERO_KEY`

    - `EXPOTIME_KEY`

    - `AIRMASS_KEY`

    - `EXTINCT_KEY`

    - `PHOTOMFLAG_KEY`

Special care has to be taken with the `CROSSID_RADIUS`. It defines the maximum distance in arcsec between two source detections. Therefore, the `CROSSID_RADIUS` divided by the time between two sub-sequent exposures sets an upper limit on the proper motion of sources that can be detected in both exposures.
Increasing the `CROSSID_RADIUS` will therefore allow for the detection of fast SSOs, however, it also increases the amount of randomly associated detections, making artifact detections more likely.
A rule of thumb: The `CROSSID_RADIUS` divided by the longest time between two-exposures (lowest upper proper motion limit) should be around 100"/h.


``semp/ssos.swarp``

    - `GAIN_KEYWORD`


After these initial changes, you should experiment with the different SExtractor, SCAMP, and pipeline settings, adjusting e.g. the filter chain parameters. A good way to fine-tune is to pick a test field with several SSOs and run the pipeline with different configurations. The cutout images will tell you what types of artifacts are remaining and whether you accidentally filtered out SSOs by restricting the candidate filters too much.


.. [#] The installation might fail if the ``pip`` tool is outdated, due to a change in the PyPI retrievals. If this is the case, run :bash:`$[sudo] pip install --upgrade pip` and repeat the install.
.. [#] The implementation does not allow for empty strings (e.g. to point to the current working directory). Instead, put the absolute path.

