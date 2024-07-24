# Minimal reproducer of py2app error

I build the Timer app with:

```
$ python setup.py py2app &> build_output.txt
```

Then attempt to run it with:

```
$ ./dist/Timer.app/Contents/MacOS/Timer &> launch_error.txt
```

This results in the error messages:

```
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
Traceback (most recent call last):
  File "<frozen zipimport>", line 576, in _get_decompress_func
ImportError: dlopen(/Users/jamie/Documents/Projects/python/DEBUG/dist/Timer.app/Contents/Resources/zlib.cpython-311-darwin.so, 0x0002): Library not loaded: @executable_path/../Frameworks/libz.1.3.1.dylib
  Referenced from: <F5AAA9A8-54AD-34C4-89D5-5A81AD8B9B32> /Users/jamie/Documents/Projects/python/DEBUG/dist/Timer.app/Contents/Resources/zlib.cpython-311-darwin.so
  Reason: tried: '/Users/jamie/Documents/Projects/python/DEBUG/dist/Timer.app/Contents/Frameworks/libz.1.3.1.dylib' (code signature invalid in <8499F826-9884-3100-AAA6-90DA9F8CB9CA> '/Users/jamie/Documents/Projects/python/DEBUG/dist/Timer.app/Contents/Frameworks/libz.1.3.1.dylib' (errno=85) sliceOffset=0x00000000, codeBlobOffset=0x00015CD0, codeBlobSize=0x00000340)
```

See [launch_error.txt](launch_error.txt) for complete error log.

Note:

* If the package is built with the `--alias` option then the launch error does not occur

* If the line `import wx` is removed from `timer.py` then the launch error does not occur

Possibly related errors in the [build output](build_output.txt):

```
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip: fatal error: the __LINKEDIT segment does not cover the end of the file (can't be processed) in: /Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app/Contents/Frameworks/libz.1.3.1.dylib

...

/Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app/Contents/MacOS/Timer: invalid or unsupported format for signature

...

/Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app/Contents/Frameworks/libz.1.3.1.dylib: main executable failed strict validation
...

/Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app/Contents/MacOS/Timer: replacing existing signature
/Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app/Contents/MacOS/Timer: invalid or unsupported format for signature
In subcomponent: /Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app/Contents/Frameworks/libz.1.3.1.dylib

...

/Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app: replacing existing signature
/Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app: resource fork, Finder information, or similar detritus not allowed
file with invalid attached data: Disallowed xattr com.apple.FinderInfo found on /Users/jamie/Documents/Projects/python/py2app-error/dist/Timer.app
```

## Version information

### Hardware

Macbook Air M1 2020, 16GB memory.

### OS

```
$ sw_vers
ProductName:  macOS
ProductVersion:  14.5
BuildVersion:  23F79
```

### Python (MacPorts)

```
$ python --version
Python 3.11.8
```

Note: Also tried using Python 3.9 with the same result.

### Pip modules

```
$ pip freeze
altgraph==0.17.4
macholib==1.16.3
modulegraph==0.19.6
numpy==2.0.1
packaging==24.1
pillow==10.4.0
py2app==0.28.8
six==1.16.0
wxPython==4.2.1
```
