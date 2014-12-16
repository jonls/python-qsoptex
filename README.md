
QSopt_ex Python bindings
========================

Usage
-----

The Python module does not yet expose the full interface of the library but
just enough is available to be able to build problems or load problems from a
file and solve it. After solving, the values of variables can be obtained, as
shown below.

These values will be returned as `fractions.Fraction` or (if the value is an
integer) `int`. Similarly, when building a problem the parameters can be given
as `fractions.Fraction` or `int` (or any other `numbers.Rational`) or anything
that can be converted to `Fraction` using the `Fraction` constructor (i.e. `float`,
`Decimal`, etc.).

``` python
import qsoptex

p = qsoptex.ExactProblem()

p.add_variable(name='x', objective=2, lower=3.5, upper=17.5)
p.add_variable(name='y', objective=-1, lower=None, upper=2)
p.add_linear_constraint(qsoptex.ConstraintSense.EQUAL, {'x': 1, 'y': 1}, rhs=0)
p.set_objective_sense(qsoptex.ObjectiveSense.MAXIMIZE)

p.set_param(qsoptex.Parameter.SIMPLEX_DISPLAY, 1)
status = p.solve()
if status == qsoptex.SolutionStatus.OPTIMAL:
    print 'Optimal solution'
    print p.get_objective_value()
    print p.get_value('x')
```

The module is also able to load problems from external files:

``` python
p = qsoptex.ExactProblem()
p.read('netlib/cycle.mps', filetype='MPS') # 'LP' is also supported
p.set_param(qsoptex.Parameter.SIMPLEX_DISPLAY, 1)
status = p.solve()
```

Building
--------

The module requires the QSopt_ex library to be installed. Currently, the
modified version at https://github.com/jonls/qsopt-ex is required.

Use `setup.py` to build the extension. The setup script is based on `distutils`.

``` shell
$ ./setup install
```

If GnuMP or QSopt_ex is installed non-standard locations, the include and library
paths can be set using the environment variables

- GnuMP: `GMP_INCLUDE_DIR` and `GMP_LIBRARY_DIR`
- QSopt_ex: `QSOPTEX_INCLUDE_DIR` and `QSOPTEX_LIBRARY_DIR`

For example, if GnuMP is installed in the `/opt/local` prefix

``` shell
$ GMP_INCLUDE_DIR=/opt/local/include GMP_LIBRARY_DIR=/opt/local/lib ./setup.py install
```
