# m00nwalk
**Flag:`picoCTF{beep_boop_im_in_space}`**
## My thought process and approach to the challenge:
Saw the hints and researched to find that `unified s band signal` was sent and then eventually converted to `standard broadcast signal` to televise the moon landing(couldn't find any online converters).  
Then did some research along with watching videos like https://www.youtube.com/watch?v=9vTrmPWULfo&t=293s&ab_channel=PrimalSpace   
Then eventually found this on wikipedia :
>The Apollo 11 missing tapes were those that were recorded from Apollo 11's slow-scan television (SSTV) telecast in its raw format on telemetry data tape at the time of the first Moon landing in 1969 and subsequently lost.

>The data tapes were used to record all transmitted data (video as well as telemetry) for backup.

>To broadcast the SSTV transmission on standard television, NASA ground receiving stations performed real-time scan conversion to the NTSC television format.

![image](https://github.com/user-attachments/assets/c9b89827-d4fd-426c-a13a-5e3c70d1c01b)

After many hours of searching for a wav to sstv converted I finally stumbled upon this : `https://github.com/colaclanth/sstv`

```bash
sambhavit@LAPTOP-JL16J5LA:~$ git clone https://github.com/colaclanth/sstv.git
Cloning into 'sstv'...
remote: Enumerating objects: 221, done.
remote: Counting objects: 100% (59/59), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 221 (delta 51), reused 49 (delta 49), pack-reused 162 (from 1)
Receiving objects: 100% (221/221), 1.01 MiB | 844.00 KiB/s, done.
Resolving deltas: 100% (139/139), done.
sambhavit@LAPTOP-JL16J5LA:~$ cd sstv
sambhavit@LAPTOP-JL16J5LA:~/sstv$ python setup.py install
Command 'python' not found, did you mean:
  command 'python3' from deb python3
  command 'python' from deb python-is-python3
sambhavit@LAPTOP-JL16J5LA:~/sstv$ python3 setup.py install
running install
/usr/lib/python3/dist-packages/setuptools/command/install.py:34: SetuptoolsDeprecationWarning: setup.py install is deprecated. Use build and pip and other standards-based tools.
  warnings.warn(
/usr/lib/python3/dist-packages/setuptools/command/easy_install.py:158: EasyInstallDeprecationWarning: easy_install command is deprecated. Use build and pip and other standards-based tools.
  warnings.warn(
error: can't create or remove files in install directory

The following error occurred while trying to add or remove files in the
installation directory:

    [Errno 13] Permission denied: '/usr/local/lib/python3.10/dist-packages/test-easy-install-843.write-test'

The installation directory you specified (via --install-dir, --prefix, or
the distutils default setting) was:

    /usr/local/lib/python3.10/dist-packages/

Perhaps your account does not have write access to this directory?  If the
installation directory is a system-owned directory, you may need to sign in
as the administrator or "root" account.  If you do not have administrative
access to this machine, you may wish to choose a different installation
directory, preferably one that is listed in your PYTHONPATH environment
variable.

For information on other options, you may wish to consult the
documentation at:

  https://setuptools.pypa.io/en/latest/deprecated/easy_install.html

Please make the appropriate changes for your system and try again.

sambhavit@LAPTOP-JL16J5LA:~/sstv$ sudo python3 setup.py install
[sudo] password for sambhavit:
running install
/usr/lib/python3/dist-packages/setuptools/command/install.py:34: SetuptoolsDeprecationWarning: setup.py install is deprecated. Use build and pip and other standards-based tools.
  warnings.warn(
/usr/lib/python3/dist-packages/setuptools/command/easy_install.py:158: EasyInstallDeprecationWarning: easy_install command is deprecated. Use build and pip and other standards-based tools.
  warnings.warn(
running bdist_egg
running egg_info
creating sstv.egg-info
writing sstv.egg-info/PKG-INFO
writing dependency_links to sstv.egg-info/dependency_links.txt
writing entry points to sstv.egg-info/entry_points.txt
writing requirements to sstv.egg-info/requires.txt
writing top-level names to sstv.egg-info/top_level.txt
writing manifest file 'sstv.egg-info/SOURCES.txt'
reading manifest file 'sstv.egg-info/SOURCES.txt'
adding license file 'LICENSE'
writing manifest file 'sstv.egg-info/SOURCES.txt'
installing library code to build/bdist.linux-x86_64/egg
running install_lib
running build_py
creating build
creating build/lib
creating build/lib/sstv
copying sstv/__main__.py -> build/lib/sstv
copying sstv/common.py -> build/lib/sstv
copying sstv/decode.py -> build/lib/sstv
copying sstv/spec.py -> build/lib/sstv
copying sstv/__init__.py -> build/lib/sstv
copying sstv/command.py -> build/lib/sstv
creating build/bdist.linux-x86_64
creating build/bdist.linux-x86_64/egg
creating build/bdist.linux-x86_64/egg/sstv
copying build/lib/sstv/__main__.py -> build/bdist.linux-x86_64/egg/sstv
copying build/lib/sstv/common.py -> build/bdist.linux-x86_64/egg/sstv
copying build/lib/sstv/decode.py -> build/bdist.linux-x86_64/egg/sstv
copying build/lib/sstv/spec.py -> build/bdist.linux-x86_64/egg/sstv
copying build/lib/sstv/__init__.py -> build/bdist.linux-x86_64/egg/sstv
copying build/lib/sstv/command.py -> build/bdist.linux-x86_64/egg/sstv
byte-compiling build/bdist.linux-x86_64/egg/sstv/__main__.py to __main__.cpython-310.pyc
byte-compiling build/bdist.linux-x86_64/egg/sstv/common.py to common.cpython-310.pyc
byte-compiling build/bdist.linux-x86_64/egg/sstv/decode.py to decode.cpython-310.pyc
byte-compiling build/bdist.linux-x86_64/egg/sstv/spec.py to spec.cpython-310.pyc
byte-compiling build/bdist.linux-x86_64/egg/sstv/__init__.py to __init__.cpython-310.pyc
byte-compiling build/bdist.linux-x86_64/egg/sstv/command.py to command.cpython-310.pyc
creating build/bdist.linux-x86_64/egg/EGG-INFO
copying sstv.egg-info/PKG-INFO -> build/bdist.linux-x86_64/egg/EGG-INFO
copying sstv.egg-info/SOURCES.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
copying sstv.egg-info/dependency_links.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
copying sstv.egg-info/entry_points.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
copying sstv.egg-info/requires.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
copying sstv.egg-info/top_level.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
zip_safe flag not set; analyzing archive contents...
creating dist
creating 'dist/sstv-0.1-py3.10.egg' and adding 'build/bdist.linux-x86_64/egg' to it
removing 'build/bdist.linux-x86_64/egg' (and everything under it)
Processing sstv-0.1-py3.10.egg
Copying sstv-0.1-py3.10.egg to /usr/local/lib/python3.10/dist-packages
Adding sstv 0.1 to easy-install.pth file
Installing sstv script to /usr/local/bin

Installed /usr/local/lib/python3.10/dist-packages/sstv-0.1-py3.10.egg
Processing dependencies for sstv==0.1
Searching for scipy
Reading https://pypi.org/simple/scipy/
/usr/lib/python3/dist-packages/pkg_resources/__init__.py:116: PkgResourcesDeprecationWarning:  is an invalid version and will not be supported in a future release
  warnings.warn(
Downloading https://files.pythonhosted.org/packages/47/78/b0c2c23880dd1e99e938ad49ccfb011ae353758a2dc5ed7ee59baff684c3/scipy-1.14.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=8e32dced201274bf96899e6491d9ba3e9a5f6b336708656466ad0522d8528f69
Best match: scipy 1.14.1
Processing scipy-1.14.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
Installing scipy-1.14.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl to /usr/local/lib/python3.10/dist-packages
Adding scipy 1.14.1 to easy-install.pth file

Installed /usr/local/lib/python3.10/dist-packages/scipy-1.14.1-py3.10-linux-x86_64.egg
Searching for numpy
Reading https://pypi.org/simple/numpy/
Downloading https://files.pythonhosted.org/packages/05/db/5d9c91b2e1e2e72be1369278f696356d44975befcae830daf2e667dcb54f/numpy-2.1.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=78574ac2d1a4a02421f25da9559850d59457bac82f2b8d7a44fe83a64f770098
Best match: numpy 2.1.3
Processing numpy-2.1.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
Installing numpy-2.1.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl to /usr/local/lib/python3.10/dist-packages
Adding numpy 2.1.3 to easy-install.pth file
Installing f2py script to /usr/local/bin
Installing numpy-config script to /usr/local/bin

Installed /usr/local/lib/python3.10/dist-packages/numpy-2.1.3-py3.10-linux-x86_64.egg
Searching for PySoundFile
Reading https://pypi.org/simple/PySoundFile/
Downloading https://files.pythonhosted.org/packages/2a/b3/0b871e5fd31b9a8e54b4ee359384e705a1ca1e2870706d2f081dc7cc1693/PySoundFile-0.9.0.post1-py2.py3-none-any.whl#sha256=db14f84f4af1910f54766cf0c0f19d52414fa80aa0e11cb338b5614946f39947
Best match: PySoundFile 0.9.0.post1
Processing PySoundFile-0.9.0.post1-py2.py3-none-any.whl
Installing PySoundFile-0.9.0.post1-py2.py3-none-any.whl to /usr/local/lib/python3.10/dist-packages
Adding PySoundFile 0.9.0.post1 to easy-install.pth file

Installed /usr/local/lib/python3.10/dist-packages/PySoundFile-0.9.0.post1-py3.10.egg
Searching for Pillow
Reading https://pypi.org/simple/Pillow/
/usr/lib/python3/dist-packages/pkg_resources/__init__.py:116: PkgResourcesDeprecationWarning: rc1 is an invalid version and will not be supported in a future release
  warnings.warn(
Downloading https://files.pythonhosted.org/packages/41/c3/94f33af0762ed76b5a237c5797e088aa57f2b7fa8ee7932d399087be66a8/pillow-11.0.0-cp310-cp310-manylinux_2_28_x86_64.whl#sha256=1a61b54f87ab5786b8479f81c4b11f4d61702830354520837f8cc791ebba0f5f
Best match: pillow 11.0.0
Processing pillow-11.0.0-cp310-cp310-manylinux_2_28_x86_64.whl
Installing pillow-11.0.0-cp310-cp310-manylinux_2_28_x86_64.whl to /usr/local/lib/python3.10/dist-packages
Adding pillow 11.0.0 to easy-install.pth file

Installed /usr/local/lib/python3.10/dist-packages/pillow-11.0.0-py3.10-linux-x86_64.egg
Searching for cffi>=0.6
Reading https://pypi.org/simple/cffi/
Downloading https://files.pythonhosted.org/packages/8d/fb/4da72871d177d63649ac449aec2e8a29efe0274035880c7af59101ca2232/cffi-1.17.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=2bb1a08b8008b281856e5971307cc386a8e9c5b625ac297e853d36da6efe9c17
Best match: cffi 1.17.1
Processing cffi-1.17.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
Installing cffi-1.17.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl to /usr/local/lib/python3.10/dist-packages
Adding cffi 1.17.1 to easy-install.pth file

Installed /usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg
Searching for pycparser
Reading https://pypi.org/simple/pycparser/
Downloading https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl#sha256=c3702b6d3dd8c7abc1afa565d7e63d53a1d0bd86cdc24edd75470f4de499cfcc
Best match: pycparser 2.22
Processing pycparser-2.22-py3-none-any.whl
Installing pycparser-2.22-py3-none-any.whl to /usr/local/lib/python3.10/dist-packages
Adding pycparser 2.22 to easy-install.pth file

Installed /usr/local/lib/python3.10/dist-packages/pycparser-2.22-py3.10.egg
Finished processing dependencies for sstv==0.1
sambhavit@LAPTOP-JL16J5LA:~/sstv$ sstv -d my_sstv.wav -o decoded_image.png
Traceback (most recent call last):
  File "/usr/local/bin/sstv", line 33, in <module>
    sys.exit(load_entry_point('sstv==0.1', 'console_scripts', 'sstv')())
  File "/usr/local/bin/sstv", line 25, in importlib_load_entry_point
    return next(matches).load()
  File "/usr/lib/python3.10/importlib/metadata/__init__.py", line 171, in load
    module = import_module(match.group('module'))
  File "/usr/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 992, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1006, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 688, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 883, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/usr/local/lib/python3.10/dist-packages/sstv-0.1-py3.10.egg/sstv/__init__.py", line 3, in <module>
  File "/usr/local/lib/python3.10/dist-packages/sstv-0.1-py3.10.egg/sstv/command.py", line 7, in <module>
  File "/usr/local/lib/python3.10/dist-packages/PySoundFile-0.9.0.post1-py3.10.egg/soundfile.py", line 267, in <module>
    _snd = _ffi.dlopen('sndfile')
  File "/usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg/cffi/api.py", line 150, in dlopen
    lib, function_cache = _make_ffi_library(self, name, flags)
  File "/usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg/cffi/api.py", line 834, in _make_ffi_library
    backendlib = _load_backend_lib(backend, libname, flags)
  File "/usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg/cffi/api.py", line 829, in _load_backend_lib
    raise OSError(msg)
OSError: ctypes.util.find_library() did not manage to locate a library called 'sndfile'
sambhavit@LAPTOP-JL16J5LA:~/sstv$ sstv -d message.wav -o decoded_image.png
Traceback (most recent call last):
  File "/usr/local/bin/sstv", line 33, in <module>
    sys.exit(load_entry_point('sstv==0.1', 'console_scripts', 'sstv')())
  File "/usr/local/bin/sstv", line 25, in importlib_load_entry_point
    return next(matches).load()
  File "/usr/lib/python3.10/importlib/metadata/__init__.py", line 171, in load
    module = import_module(match.group('module'))
  File "/usr/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 992, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1006, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 688, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 883, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/usr/local/lib/python3.10/dist-packages/sstv-0.1-py3.10.egg/sstv/__init__.py", line 3, in <module>
  File "/usr/local/lib/python3.10/dist-packages/sstv-0.1-py3.10.egg/sstv/command.py", line 7, in <module>
  File "/usr/local/lib/python3.10/dist-packages/PySoundFile-0.9.0.post1-py3.10.egg/soundfile.py", line 267, in <module>
    _snd = _ffi.dlopen('sndfile')
  File "/usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg/cffi/api.py", line 150, in dlopen
    lib, function_cache = _make_ffi_library(self, name, flags)
  File "/usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg/cffi/api.py", line 834, in _make_ffi_library
    backendlib = _load_backend_lib(backend, libname, flags)
  File "/usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg/cffi/api.py", line 829, in _load_backend_lib
    raise OSError(msg)
OSError: ctypes.util.find_library() did not manage to locate a library called 'sndfile'
sambhavit@LAPTOP-JL16J5LA:~/sstv$ sudo apt update
sudo apt install libsndfile1
Hit:1 http://archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [129 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [128 kB]
Get:4 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1906 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [127 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [2127 kB]
Get:7 http://security.ubuntu.com/ubuntu jammy-security/main Translation-en [307 kB]
Get:8 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [912 kB]
Get:9 http://security.ubuntu.com/ubuntu jammy-security/universe Translation-en [181 kB]
Ign:6 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages
Get:10 http://archive.ubuntu.com/ubuntu jammy-updates/main Translation-en [365 kB]
Get:11 http://archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [2605 kB]
Get:12 http://archive.ubuntu.com/ubuntu jammy-updates/restricted Translation-en [450 kB]
Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1134 kB]
Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/universe Translation-en [265 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [2127 kB]
Fetched 8509 kB in 45s (190 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
2 packages can be upgraded. Run 'apt list --upgradable' to see them.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages were automatically installed and are no longer required:
  htop libnl-3-200 libnl-genl-3-200
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  libflac8 libogg0 libopus0 libvorbis0a libvorbisenc2
Suggested packages:
  opus-tools
The following NEW packages will be installed:
  libflac8 libogg0 libopus0 libsndfile1 libvorbis0a libvorbisenc2
0 upgraded, 6 newly installed, 0 to remove and 2 not upgraded.
Need to get 715 kB of archives.
After this operation, 2226 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://archive.ubuntu.com/ubuntu jammy/main amd64 libogg0 amd64 1.3.5-0ubuntu3 [22.9 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libflac8 amd64 1.3.3-2ubuntu0.2 [111 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy/main amd64 libopus0 amd64 1.3.1-0.1build2 [203 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy/main amd64 libvorbis0a amd64 1.3.7-1build2 [99.2 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy/main amd64 libvorbisenc2 amd64 1.3.7-1build2 [82.6 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 libsndfile1 amd64 1.0.31-2ubuntu0.1 [197 kB]
Fetched 715 kB in 9s (83.9 kB/s)
Selecting previously unselected package libogg0:amd64.
(Reading database ... 54616 files and directories currently installed.)
Preparing to unpack .../0-libogg0_1.3.5-0ubuntu3_amd64.deb ...
Unpacking libogg0:amd64 (1.3.5-0ubuntu3) ...
Selecting previously unselected package libflac8:amd64.
Preparing to unpack .../1-libflac8_1.3.3-2ubuntu0.2_amd64.deb ...
Unpacking libflac8:amd64 (1.3.3-2ubuntu0.2) ...
Selecting previously unselected package libopus0:amd64.
Preparing to unpack .../2-libopus0_1.3.1-0.1build2_amd64.deb ...
Unpacking libopus0:amd64 (1.3.1-0.1build2) ...
Selecting previously unselected package libvorbis0a:amd64.
Preparing to unpack .../3-libvorbis0a_1.3.7-1build2_amd64.deb ...
Unpacking libvorbis0a:amd64 (1.3.7-1build2) ...
Selecting previously unselected package libvorbisenc2:amd64.
Preparing to unpack .../4-libvorbisenc2_1.3.7-1build2_amd64.deb ...
Unpacking libvorbisenc2:amd64 (1.3.7-1build2) ...
Selecting previously unselected package libsndfile1:amd64.
Preparing to unpack .../5-libsndfile1_1.0.31-2ubuntu0.1_amd64.deb ...
Unpacking libsndfile1:amd64 (1.0.31-2ubuntu0.1) ...
Setting up libogg0:amd64 (1.3.5-0ubuntu3) ...
Setting up libflac8:amd64 (1.3.3-2ubuntu0.2) ...
Setting up libopus0:amd64 (1.3.1-0.1build2) ...
Setting up libvorbis0a:amd64 (1.3.7-1build2) ...
Setting up libvorbisenc2:amd64 (1.3.7-1build2) ...
Setting up libsndfile1:amd64 (1.0.31-2ubuntu0.1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.8) ...
sambhavit@LAPTOP-JL16J5LA:~/sstv$ ldconfig -p | grep sndfile
        libsndfile.so.1 (libc6,x86-64) => /lib/x86_64-linux-gnu/libsndfile.so.1
sambhavit@LAPTOP-JL16J5LA:~/sstv$ pip3 uninstall soundfile
pip3 install soundfile
WARNING: Skipping soundfile as it is not installed.
Defaulting to user installation because normal site-packages is not writeable
Collecting soundfile
  Downloading soundfile-0.12.1-py2.py3-none-manylinux_2_31_x86_64.whl (1.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 478.0 kB/s eta 0:00:00
Requirement already satisfied: cffi>=1.0 in /usr/local/lib/python3.10/dist-packages/cffi-1.17.1-py3.10-linux-x86_64.egg (from soundfile) (1.17.1)
Requirement already satisfied: pycparser in /usr/local/lib/python3.10/dist-packages/pycparser-2.22-py3.10.egg (from cffi>=1.0->soundfile) (2.22)
Installing collected packages: soundfile
Successfully installed soundfile-0.12.1
sambhavit@LAPTOP-JL16J5LA:~/sstv$ sstv -d message.wav -o decoded_image.png
usage: sstv [-h] [-d AUDIO_FILE] [-o OUTPUT_FILE] [-s SKIP] [-V] [--list-modes] [--list-audio-formats] [--list-image-formats]
sstv: error: argument -d/--decode: can't open 'message.wav': [Errno 2] No such file or directory: 'message.wav'
sambhavit@LAPTOP-JL16J5LA:~/sstv$ pwd
/home/sambhavit/sstv
sambhavit@LAPTOP-JL16J5LA:~/sstv$ ls
LICENSE  README.md  build  dist  examples  setup.py  sstv  sstv.egg-info  test
sambhavit@LAPTOP-JL16J5LA:~/sstv$ sstv -d /home/sambhavit/sstv/message.wav -o decoded_image.png
usage: sstv [-h] [-d AUDIO_FILE] [-o OUTPUT_FILE] [-s SKIP] [-V] [--list-modes] [--list-audio-formats] [--list-image-formats]
sstv: error: argument -d/--decode: can't open '/home/sambhavit/sstv/message.wav': [Errno 2] No such file or directory: '/home/sambhavit/sstv/message.wav'
sambhavit@LAPTOP-JL16J5LA:~/sstv$ sstv -d /home/sambh/sstv/message.wav -o decoded_image.png
usage: sstv [-h] [-d AUDIO_FILE] [-o OUTPUT_FILE] [-s SKIP] [-V] [--list-modes] [--list-audio-formats] [--list-image-formats]
sstv: error: argument -d/--decode: can't open '/home/sambh/sstv/message.wav': [Errno 2] No such file or directory: '/home/sambh/sstv/message.wav'
sambhavit@LAPTOP-JL16J5LA:~/sstv$ sstv -d /mnt/c/Users/sambh/Downloads/message.wav -o decoded_image.png
[sstv] Searching for calibration header... Found!
[sstv] Detected SSTV mode Scottie 1
[sstv] Decoding image...                         [####################################################################################################] 100%
[sstv] Drawing image data...
[sstv] ...Done!
sambhavit@LAPTOP-JL16J5LA:~/sstv$ ls
LICENSE  README.md  build  decoded_image.png  dist  examples  setup.py  sstv  sstv.egg-info  test
sambhavit@LAPTOP-JL16J5LA:~/sstv$ explorer.exe decoded_image.png
```
This gave me the `decoded_image.png`


![decoded_image](https://github.com/user-attachments/assets/5fbbaee4-3f72-4ef9-b3a5-0d380ab59348)



which in turn helped me get the flag `picoCTF{beep_boop_im_in_space}`
(also found out the CMU mascot was Scotty, which is the mode but since it was default I didn't need to change it lol)

## Every single new concept and point of knowledge I learned or improved upon through solving the challenge.
1)Learnt about the `steghide` tool and how to run it.(did it for the first time)    
2)Learnt about wav to sstv converters and also that audio files can be converted into images(crazy).  
3)Would say I improved a lot in decoding audio , before this I would've quit if it wasn't the easiest answer(i.e Morse code).  
4)Refined my skills of Installation and Usage of Github Programs.  

##  Incorrect tangents I went on while solving :
1) Since I had an audio file(wav) , first I tried using an online audio decoder to convert it to Morse Code thinking I could get a flag but just got random letters that I couldn't make sense of.  
2) Learnt and tried using the `steghide` tool with linux to find any hidden data but couldn't figure out the passphrase(trust me I tried all moon related passphrases)  
   ![image](https://github.com/user-attachments/assets/46ac28f9-7bc7-43dd-83e8-531bbb04798a)  
3)Tried to analyse the waveform of the audio in Audible to try to find a hidden code or pattern of sorts.  
