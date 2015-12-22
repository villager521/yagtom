**Matlab vs Octave**


[Octave](http://www.gnu.org/software/octave/) is a free version of Matlab.
However, there are some reasons to prefer Matlab:

  * It has much better graphics
  * It has a much better IDE (integrated development environment), including an excellent editor, debugger, and profiler
  * It is faster than Octave, since it has a just-in-time compiler.
  * It has more functions.

There are two main cases when you may need the free version: when running code on the cloud, and when you want to make your code runnable by general users who may not want to buy matlab.
For the former, it is best to use the [matlab compiler](http://www.mathworks.com/products/compiler/index.html), which lets you generate stand-alone code. (Note this code is not generally faster than interpreted code; basically the stand-alone code contains a matlab virtual machine inside it.)
For the latter, you may want to use octave. Unfortunately, converting code from matlab to octave is not entirely pain-free, for reasons we explain below.

**Compatability of Octave and Matlab**

  * Octave's legend function has a different interface than Matlab's.

  * Octave sets random seeds differently than Matlab, hence random data is not consistent.

  * Octave does not yet (Fall 2011) support Matlab's Object Oriented System and has only limited support for the old style OO system, (e.g. no operator overloading support).

  * Octave does not support the concatenation of a cell array of strings and a string, you must first wrap the string in a cell.

```
[{'a', 'b'}, 'c'] % works in Matlab, not in Octave
[{'a', 'b'}, {'c'}] % works in both with the same results as above
```

  * Vertical concatenation of nested cell arrays of strings, e.g. vertcat(s{:}) behaves differently in Matlab and Octave. Matlab returns a cell array of strings, Octave returns a character array.

  * Calling bsxfun(@eq, a(:), sparse(1:n)) fails in Octave, but works if you omit the call to sparse.

  * The octave `which` function behaves slightly differently than Matlab's If you call `w = which('foo.m')` in Octave while in the same directory as `foo.m`, octave returns `w = '.\foo.m'`, whereas Matlab will return the full absolute filepath. This causes problems when calling `addpath`.

  * The following command fails, (does nothing) in Octave: `addpath(genpath(pwd), '-end')`. The problem is the `'-end'` option.

  * [Here](http://wiki.octave.org/wiki.pl?MissingMatlabFunctions) is a list of files in Matlab 2008a, missing in Octave.

  * Octave does not yet support Matlab's new Object Oriented System and has only limited support for the old style OO system, (e.g. no operator overloading support). This should have no impact on [PMTK3](http://code.google.com/p/pmtk3/), which no longer uses objects. However, PMTK1 and PMTK2 will not work in Octave, and nor will  [graphviz4matlab](http://code.google.com/p/graphviz4matlab).

  * The 'rows' function in Minka's [lightspeed](http://research.microsoft.com/en-us/um/people/minka/software/lightspeed/) toolbox masks the built in Octave 'rows' function, which causes subtle bugs in unexpected places.

  * Matlab uses different file extensions for different versions of .mex files, depending on the operating system. Octave does not, so it is difficult to include pre-compiled versions for multiple machines. See [this page](http://www.gnu.org/software/octave/doc/interpreter/Dynamically-Linked-Functions.html#Dynamically-Linked-Functions) for more details.


**Running Octave from within Matlab**

Octave can be launched within the Matlab command window, (useful for development purposes) by using the following code. Add these lines to a Matlab shortcut for single button access.

```
addtosystempath('C:\Octave\3.2.3_gcc-4.4.0\bin');
system('octave --traditional --persist --silent -i -p C:\pmtkData --eval addpath(genpath(''C:\pmtk3''))');

```

Type ` exit() ` to return to Matlab.

Code in the Matlab editor can be executed in Octave, (while its running) by highlighting it and pressing the ` F9 ` key.

**Additional links**

  * http://www.cs.toronto.edu/~murray/compnotes/octave_matlab.html
  * http://www.gnu.org/software/octave/FAQ.html#MATLAB-compatibility.