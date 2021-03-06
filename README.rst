=============================
PYADOLC, a wrapper for ADOL-C
=============================

Short Description:
    This PYADOLC, a Python module to differentiate complex algorithms written in Python.
    It wraps the functionality of the library ADOL-C (C++).

Author:
    Sebastian F. Walter

Licence (new BSD):
    Copyright (c) 2008, Sebastian F. Walter
    All rights reserved.
    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions are met:
    * Redistributions of source code must retain the above copyright
    notice, this list of conditions and the following disclaimer.
    * Redistributions in binary form must reproduce the above copyright
    notice, this list of conditions and the following disclaimer in the
    documentation and/or other materials provided with the distribution.
    * Neither the name of the HU Berlin nor the
    names of its contributors may be used to endorse or promote products
    derived from this software without specific prior written permission.

    THIS SOFTWARE IS PROVIDED BY Sebastian F. Walter ''AS IS'' AND ANY
    EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
    WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
    DISCLAIMED. IN NO EVENT SHALL Sebastian F. Walter BE LIABLE FOR ANY
    DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
    (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
    LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
    ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
    (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
    SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


EXAMPLE USAGE::

    import numpy
    from adolc import *
    N = M = 10
    A = numpy.zeros((M,N))
    A[:] = [[ 1./N +(n==m) for n in range(N)] for m in range(M)]
    def f(x):
        return numpy.dot(A,x)

    # tape a function evaluation
    ax = numpy.array([adouble(0) for n in range(N)])
    trace_on(1)
    independent(ax)
    ay = f(ax)
    dependent(ay)
    trace_off()

    x = numpy.array([n+1 for n in range(N)])

    # compute jacobian of f at x
    J = jacobian(1,x)

    # compute gradient of f at x
    if M==1:
        g = gradient(1,x)


REQUIREMENTS:
    * Known to work for Ubuntu Linux, Python 2.6, NumPy 1.3.0, Boost:Python 1.40.0
    * Python and Numpy, both with header files
    * ADOL-C version 2.1.0 http://www.coin-or.org/projects/ADOL-C.xml
    * boost::python from http://www.boost.org/

OPTIONAL REQUIREMENTS:
    * For sparse Jacobians and Hessians: ColPack 1.0.0 http://www.cscapes.org/coloringpage/software.htm
    * scons build tool (makes things easier if you need to recompile pyadolc)

INSTALLATION:

    * CHECK REQUIREMENTS: Make sure you have ADOL-C (version 2.1 and above), ColPack (version 1.0.0 and above) the boost libraries and numpy installed. All with header files.
    * BUILD COLPACK
        * run ``make``
        * this should generate ``~workspace/ColPack/build/lib/libColPack.so``.
    * BUILD ADOL-C:
        * run ``./configure --enable-sparse --with-colpack=/home/b45ch1/workspace/ColPack/build``
        * REMARK: the option ``--enable-sparse`` is used in ADOLC-2.2.1. In ADOLC-2.1.0 it is called ``--with-sparse``.
        * run ``make``
        * You don't have to run ``make install``.
        * You should then have a folder ``~/workspace/ADOL-C-2.1.0/ADOL-C`` with  ``adolc/adolc.h`` in it.
    * DOWNLAD PYADOLC: ``cd ~`` and then ``git clone git://github.com/b45ch1/pyadolc.git``
    * BUILD PYADOLC:
        * change to the  folder ``~/pyadolc`` and rename the file ``setup.py.EXAMPLE`` to ``setup.py``.
        * Adapt ``setup.py`` to fit your system. In particular, you have to set the paths to your ADOL-C installation and boost python.
        * run ``python setup.py build``. A new folder with a name similar to ``~/pyadolc/build/lib.linux-x86_64-2.6`` should be generated.
        * run ``python setup.py install`` to install pyadolc to your system.
    * TEST YOUR INSTALLATION:
        * Change directory to ``~/pyadolc/build/lib.linux-x86_64-2.6``
        * run ``python -c "import adolc; adolc.test()"``. All tests should pass.
    * You can also use scons (if you have it) instead of using setup.py
    * If anything goes wrong, please file a bug report.

