# ADFC (CFD) on RevyOS

## Obtain the Source Code

```
curl -L "https://sourceforge.net/projects/adfc/files/adfc/testing/adfc-3.13-devel.zip/download" -oadfc-3.13-devel.zip
unzip adfc-3.13-devel.zip
```

## Compile and Install

Can't compile cpp code with modern g++

```
debian@revyos-pioneer:~/CFD/adfc-3.13/adfc$ make
g++ -O3 -DNDEBUG -c backup.cpp
In file included from adfc.h:54,
                 from backup.cpp:11:
array.h: In member function 'void Array<tipo>::truncate(unsigned int)':
array.h:467:17: error: 'cerr' was not declared in this scope; did you mean 'std::cerr'?
  467 |                 cerr << "Array<type>:truncate(...): error, only truncation is implemented.\n";
      |                 ^~~~
      |                 std::cerr
In file included from adfc.h:11:
/usr/include/c++/14/iostream:64:18: note: 'std::cerr' declared here
   64 |   extern ostream cerr;          ///< Linked to standard error (unbuffered)
      |                  ^~~~
adfc.h:45:27: error: 'endl' was not declared in this scope; did you mean 'std::endl'?
   45 | #define WHERE_I_AM  cerr<<endl<<__FILE__<<":["<<__LINE__<<"]"<<endl;
      |                           ^~~~
array.h:469:17: note: in expansion of macro 'WHERE_I_AM'
  469 |                 WHERE_I_AM;
      |                 ^~~~~~~~~~
In file included from /usr/include/c++/14/iostream:41:
/usr/include/c++/14/ostream:744:5: note: 'std::endl' declared here
  744 |     endl(basic_ostream<_CharT, _Traits>& __os)
      |     ^~~~
adfc.h: At global scope:
adfc.h:758:88: error: 'void ConjugatedGradient::multiplyByMInverse(Array<double>&, Array<double>&)' is private within this context
  758 |         friend void ConjugatedGradient::multiplyByMInverse(Array<DBL> &a, Array<DBL> &b);
      |                                                                                        ^
adfc.h:348:14: note: declared private here
  348 |         void multiplyByMInverse(Array<DBL> &, Array<DBL> &);
      |              ^~~~~~~~~~~~~~~~~~
adfc.h:759:57: error: 'void ConjugatedGradient::preconditionate()' is private within this context
  759 |         friend void ConjugatedGradient::preconditionate();
      |                                                         ^
adfc.h:350:14: note: declared private here
  350 |         void preconditionate();
      |              ^~~~~~~~~~~~~~~
make: *** [Makefile:33: backup.o] Error 1
```

