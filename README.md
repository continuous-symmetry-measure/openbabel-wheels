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
This is a step-by-step description on what we've done with the Python 3.10 wheel. We did exactly the same with the other versions.

1. Copy the openbabel-3.1.1-cp310-cp310-win_amd64.whl to rebrand/win-3.10
2. Unzip the file into the same directory
3. The result was several directories all with names starting with openbabel. We added csm_ to the beginning of each directory name, resulting in directories that all start with csm_openbabel.
4. We updated csm_openbabel-3.1.1.dist-info/METADATA: changed the package name to csm_openbabel and added a note to the description as follows:


NOTE: This is a repackaged version of openbabel-3.1.1 - it is identical 
with the exception of the name. We rebranded it because there was no wheel
uploaded for Python 3.8-3.10.

5. Zipped all the csm_openbabel* folders to a file called csm_openbabel-3.1.1-cp310-cp310-win_amd64.whl and uploaded to pypi.

Note - we kept the exact wheel name as before, only added csm_ to its start. This is important, as different Python versions may have different file naming conventions for wheel files.
