
## To build:

for the impatient:
```
cd src; make;
```
for the slightly less impatient: 
```
cd src; make PLATFORM=ARCH;
```

NOTE: The build system currently requires `GNU make`.  This can usually be called
as `make` or `gmake`.

There are several makefile stubs in `make/`.  You can look for your architecture
there or roll your own.  Typing 'make' (or possibly gmake) without any arguments 
will cause the build program to look for `uname -s`.mk(for example, on Linux, it 
will look for Linux.mk).  You can choose the platform by typing `make PLATFORM=arch`.

There are various variables to set in the .mk files.  The most important are
CXX and CXXFLAGS; the rest are performance and debugging-related(these are 
also very important in large calculations!)  See make/Linux.mk for an example
of a fairly well-optimized .mk file.

## Makefile variables:

* `CXX`: C++ compiler.  For example, for a serial compile, set this to g++.  For
parallel, set it to mpiCC or mpicxx (or mpic++).

* `CXXFLAGS`: Flags to pass to the compiler.  Put optimization (-O2) flags, etc
here.  Must also include the option "$(INCLUDEPATH)"

* `DEBUG`: Define any debugging flags here.  An optimized DEBUG might be
`-DNDEBUG`.  A safe and slow DEBUG could be 
`-DDEBUG_WRITE -DRANGE_CHECKING`

* `LDFLAGS`: Linker flags.  Usually you don't need anything here, but some 
compilers need '-lm' for the math libraries.

* `BLAS_LIBS`: The BLAS libraries(for example: -L/usr/lib -lcblas

* `LAPACK_LIBS`: LAPACK libs(as BLAS) (ex. -L/opt/lapack/lib -llapack)

* `LAPACK_INCLUDE`: LAPACK headers (for examples -I/opt/lapack/

* `EINSPLINE_LIBS`: Einspline libs

* `EINSPLINE_INCLUDE`: Einspline include directory


* `DEPENDMAKER`: If you have gcc, it should be g++ -MM -I $(INCLUDEPATH).


List of preprocessor flags(these may be defined by -D[FLAG] with most
compilers.  List them in the CXXFLAGS variable along with any optimization
flags.)

DEBUG_WRITE: Enable extra debugging output.

RANGE_CHECKING: Enable range checking of arrays.  Big performance hit if you
turn this on!

NDEBUG: Turn off assert()'s in code.  For less safety and better performance
set this flag.

USE_BLAS: Enable usage of CBLAS libraries.  This will enable BLAS in some
of the linear algebra routine and enable BLAS_MO, which is a very fast MO
evaluator for some archetectures(Itanium is one that really benefits from this).
You must also set the BLAS_LIBS and BLAS_INCLUDE variables.

USE_LAPACK:  Enables usage of LAPACK libraries in pw2lcao.  It's not used
in the main program, but improves pw2lcao's performance by several orders of
magnitude.  You must also set the LAPACK_LIBS and BLAS_INCLUDE variables.

USE_MPI:  Enable use of MPI parallelization.  For large calculations, this is
quite necessary.

USE_EINSPLINE: Use the einspline libraries from Ken Esler to evaluate blips.  EINSPLINE_LIBS and EINSPLINE_INCLUDE must also be set.
