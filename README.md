[logo]: https://raw.github.com/marcodebe/dicomecg_convert/master/images/logo.png
![ECG Dicom Convert][logo]

Dicom ECG Conversion
====================
Convert Dicom ECG (waveform) to PDF, PNG, etc.

Usage
-----
```bash
python ecgconvert.py input.dcm -o ouput.pdf
```

The ouput format is deduced from the extension of the filename.

Supported output formats are: eps, jpeg, jpg, pdf, pgf, png, ps, raw, rgba,
svg, svgz, tif, tiff.

The sample file is a 12-lead ECG dicom file produced by Mortara equipment.

The signals are filtered using a bandpass (0.05-40 Hz) butterworth filter of order 1.

Work in progress, we need:
 * print textual info (patient and wave info)
 * different layouts
 * exception handling
 * ...

Install
-------
The python library dependencies are:
* dicom
* numpy
* matplotlib
* scipy

You can install the corresponding packages from your distribution or in a virtualenv.

### Without virtualenv
```bash
sudo apt-get install python-matplotlib python-dicom python-scipy python-numpy
git clone git@github.com:marcodebe/dicomecg_convert.git
```

### Inside a virtualenv

Installing the dependencies inside the virtualenv could be long and not smooth.
I had to install system libraries and a fortran compiler.

```bash
git clone git@github.com:marcodebe/dicomecg_convert.git
virtualenv dicomecg_convert
. dicomecg_convert/bin/activate
sudo apt-get install libblas-dev
sudo apt-get install liblapack-dev 
sudo apt-get install gfortran
pip install pydicom
pip install numpy
pip install matplotlib
pip install cython
pip install git+http://github.com/scipy/scipy/
```

Structure of DICOM Waveform
---------------------------
```
       Series
          |
 contains | (1,n)
          |
          |
          v
       Waveform         * Waveform Attributes
          |                 Time of Acquisition
 contains | (1,n)           Acquisition Context
          |                 Annotation
          |
          v
    Multiplex Group     * Multiplex Group Attributes
          |                 Number of Channels
 contains | (1,n)           Sampling Frequenciy
          |                 Timing
          |
          v
       Channel          * Channel Definition Attributes
          |                 Channel Source
          |                   Metric
          |                   Anatomic Location(s)
          |                   Function
 contains | (1,n)             Tecnique
          |                 Channel Sensitivity
          |                 Baseline
          |                 Skew
          |                 Filter Characteristics
          |
          v
        Sample

```
