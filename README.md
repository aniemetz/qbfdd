  QBFDD
===============================================================================

QBFDD is a delta debugger for Quantified Boolean Formulas (QBF) in QDIMACS
format. It serves as an input minimizer for QBF solvers failing on some given
input. For more details on the QDIMACS format see [1]. For more details on
QBF in general see [2].

QBFDD is written in Python 3 and developed on a Linux OS.

QBFDD is free software released under the GPLv3. You should have received a
copy of the license in the file COPYING.  
If not, see [3].


 Download:
-------------------------------------------------------------------------------

  The latest version of QBFDD can be found on GitHub:
  https://github.com/aniemetz/qbfdd


 Usage:
-------------------------------------------------------------------------------

QBFDD minimizes failure-inducing input in QDIMACS format based on the exit
code and the outpput (stdout and stderr) of a given command when executed
on that input.

Note that the input file does not have to be strictly QDIMACS standard
compliant but has to obey the following basic rules:  

  - basic structure:  

      - unspecified number of comments of the form: ``c <string>``  
      - header: ``p cnf <pnum> <pnum> 0`` with ``pnum > 0``  
      - quantifier sets (quantifiers must be either ``e`` or ``a``) 
      - clauses  

  - all input (comments, header, quantifier sets and clauses) must follow
    the grammatical structure defined in QDIMACS standard version 1.1
    (see http://www.qbflib.org/qdimacs.html#input)  

  - given quantifier sets must be alternating  

  - empty quantifier sets are not allowed  

  - given number of clauses and number of clauses given in the header
    must match  


#### Invocation:

```
/path/to/qbfdd.py [options] INFILE "CMD [options]"
```

#### Positional arguments:

```
INFILE            is the input file to be minimized and  
"CMD [options]"   the executable to minimize given input on, options to this executable included  
```

#### Optional arguments:
  
```
--version    Show QBFDD's version number and exit.  
-h, --help   Show a brief help msg and exit.

-f value, --failed=value       With these two options it is possible to define expected return 
                               values of the given executable for failed and passed test runs.
-p value, --passed=value       Note: A test run may FAIL or PASS on given input. We are looking
                               for test runs that FAIL (as this is the original behaviour of the
                               solver on the given input). If a test run FAILs, the respective
                               minimization step was SUCCESSFUL, and if it PASSes, it wasn't.

-o filename, --out=filename    Specify an output filename to be used, else the following is used
                               by default: <INPUT FILE NAME>_reduced.<EXTENSION>
                               Note, that multiple dots as part of the input file name as well
                               as file names not defining any file extension are allowed. 

-O filename, --tmp=filename    Specify a filename for the temporary work file, else the
                               following is used by default: tmp-<PID>.qdimacs

-m mode, --mode=mode           Specify the minimization strategy to be used.
                               By default, A. Zeller's 'ddmin' is used (see A.  Zeller,
                               R. Hildebrandt: Simplifying failure-inducing input, ISSTA 2000).
                               Other modes currently supported:
                                 - sddmin (simple ddmin, see the ddmin algorithm in A. Zeller:
                                           Why programs fail, ISBN 3-89864-279-8, dpunkt Verlag, 2006)
                                 - iddmin (inverse ddmin)
                                 - isddmin (inverse sddmin)
                                 - obo (one by one, with restarts)
                                 - qobo (quick obo, without restarts)
        
-g level, --gran=level         Specify the minimization granularity to be used.
                                 - c : remove CLAUSES only
                                       Minimizing clauses (removing literals) is skipped.
                                 - l : remove LITERALS only
                                       Clauses are minimized, but not removed (the above step is skipped).
                                 - b : BOTH
                                       First, remove as many clauses as possible. Then remove as
                                       many literals as possible.

-s, --shift                    Enable quantifier manipulations (disabled by default), in order
                               to maybe accomplish even further minimizations.

-S, --skip-output              Do not consider output of the given executable but base results
                               of test runs on the return code only, disabled by default.

-t value, --timeout=value      Specifiy a time out for test runs, disabled by default.

-q, --no-qdimacs               Do not care about QDIMACS standard compliance (both for
                               minimization and output file format), disabled by default.

-c, --no-color                 Disable colored output for verbose messages.

-v, --verbose                  Increase verbosity.

```


  References: 
-------------------------------------------------------------------------------
  
  [1] http://www.qbflib.org/qdimacs.html  
  [2] http://www.qbflib.org  
  [3] http://www.gnu.org./licenses  
