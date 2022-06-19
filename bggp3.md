# bggp3

> The goal of the [3rd Annual Binary Golf Grand Prix (BGGP3)](https://tmpout.sh/bggp/3/) is to find the smallest file which will crash (or hang) a specific program.

This document in markdown format is available [here](bggp3.md).

## entries
### gnu m4
gnu m4 is a macro processing language and tool. it is widely used in open source projects, especially in conjunction with `autoconf` to prepare source to be compiled on a given system.

by defining a recursive macro, we can send it into an infinite loop, which the OS kills

**[3.m4](3.m4)**:
```
define(`a',`a 3')
a.
```

**result**:
```
$ m4 3.m4

Killed
$
```

## fuzzing tech
### afl++
#### non-instrumented

```
afl-fuzz -Q -D -i in -o out -- TARGET -FLAGS @@
```

#### instrumented
```
./configure CC="afl-gcc" CXX="afl-g++" --disable-shared
make
afl-fuzz -D -i in -o out -- TARGET -FLAGS @@
```

### honggfuzz
#### non-instrumented

```
honggfuzz -n NUMTHREADS -i ./inputs/ -x -- TARGET -FLAGS ___FILE___
```

#### instrumented

```
./configure CC="hfuzz-gcc" CXX="hfuzz-g++" --disable-shared
make
honggfuzz -n NUMTHREADS -i ./inputs/ -x -- TARGET -FLAGS ___FILE___
```