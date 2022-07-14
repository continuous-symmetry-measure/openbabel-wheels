# opebabel-wheels
This repository contains the Python Wheels for Windows for Open Babel 3.1.1

For some reason, the OpenBabel in pypi contains wheels for Python 3.7, but not for later versions. The wheels can be found in Christoph Gohlke's Windows wheels website, but they are not downloadable by pip from there.

So we took those wheels and rebranded them as csm-openbabel, and uploaded back to pypi.

These repackaged Openbabel wheels have the same license as Openbabel. See the Openbabel documentation for more information.

## The Repackaging Process

We repackaged the source distribution as well as the Windows wheels. First we downloaded all the wheels from Pypi (Windows Python 3.7) and Christoph Gohlke's site (Windows Python 3.8-3.10), as well as the source package (from pypi) and placed them all in the wheels folder of this project.

We then converted them one by one.

### Source package
To convert the source package, we opened the openbabel-3.1.1.1.tar.gz file into the rebrand/source directory. We renamed it csm_openbabel-3.1.1, updated __init__.py which contains the version to 3.1.1. We also updated setup.py's package name to csm_openbabel, and also the PKG_INFO file.

We changed the version number because OpenBabel 3.1.1 has Windows wheels (for Python 3.7) and no source distribution. OpenBabel 3.1.1.1 is a source-only distribution. We wanted to combine both to a single package.

We tar -czvf csm_openbabel-3.1.1.tar.gz csm_openbabel-3.1.1/ (from WSL) and uploaded to pypi

### Windows Wheels
The Windows wheels are all based on the Windows 3.7 wheel created by the OpenBabel team, and not the Christophe Gholke's wheels. His wheels contain their own non-working version of openbabel, which we had to remove.

For Python 3.7, we unzipped the Python 3.7 wheel from `wheels/openbabel-3.1.1-cp37-cp37m-win64_amd.whl` and:

1. Rename openbabel-3.1.1.dist-info to csm_openbabel-3.1.dist-info
2. Update the METADATA file in the above `dist` folder - by changing the version number and adding a comment as follows:

NOTE: This is a repackaged version of openbabel-3.1.1 - it is identical 
with the exception of the name and version number. We rebranded it because there was no wheel
uploaded for Python 3.8-3.10.

3. Zip everything into a new wheel file.

Other Python versions were a bit trickier. The Christophe Ghokle wheels contain their own version of openbabel which just doesn't work (it doesn't find many file formatters). The solution was to take the Python 3.7 wheel created above and adapt it a little bit.

1. Unzip the Christophe Gholke wheel to a temp directory, and copy the openbabel/_openabel.cp3??-win_amd64.pyd file to a side directory.
2. Unzip the Python 3.7 wheel into `win-3.*`
3. Remove the file _openbabel.py from the top directory, and copy the pyd file you kept aside into the `openbabel` directory.
4. Update the RECORD file in the dist folder to reflect the new file (this is optional, pip pretty much ignores this file)
5. Zip

## Differences between csm_openbabel and openbabel
For Python 3.7 on Windows, and for all Linux versions, the two packages are identical. You don't need to change anything, and they will work.

However, on Windows and Python 3.8 or later, you need to add an extra step before you can use openbabel. If you just import openbabel as usual, you will get an import error, saying a required module cannot be found. This is the openbabel-3.dll file. It cannot be found because starting with Python 3.8 on Windows, Python does not look for DLLs on the system path, but only in specific places. You need to call `os.add_dll_directory` with the right directory for the import to work.

The CSM source code has such a function you can use.

Note: The Christophe Gohlke wheels included their own openbabel version, but it was only partly working. It did not support various formats (such as pdb), because the formatters weren't loaded - not from the included version nor from the installed version.
