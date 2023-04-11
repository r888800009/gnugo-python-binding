# GNU Go Python Binding

This repository provides [gnugo](https://www.gnu.org/software/gnugo/) API for python. Compared with using pure python, this binding makes the python Go application closer to the performance of native code.

In addition, python binding can avoid IPC overhead, such as using popen to wrap gnugo and communicate with python applications through [gtp protocol](https://www.gnu.org/software/gnugo/gnugo_19.html), 
Actually running two Processes, the middle process will reduce performance.

gnugo python binding can load the gnugo library into the same address space to improve performance.
This project mainly uses [pyclibrary](https://github.com/MatthieuDartiailh/pyclibrary) to translate c header and bind.

Note:

This library is a third-party library and is not part of the official GNU GO.
The [Free Software Foundation](https://www.fsf.org/) owns the copyright of GNU GO.
Problems caused by using third-party libraries may not be officially supported,
and the use needs to be considered [Restrictions](https://www.gnu.org/software/gnugo/gnugo_1.html#SEC3)

# Dependency
for build
```
clang and gcc
```

for gnugo
```
# linux
sudo pacman -S ncurses
sudo apt install libncurses-dev

# macos
brew install ncurses
```
Also following [GNU Go Documentation: 2. Installation](https://www.gnu.org/software/gnugo/gnugo_2.html#SEC8)

python binding
```
python3.10
pyclibrary
requests
```

Also see [requirements.txt](./requirements.txt)

# Build
`GNUGoWrapper` will use the functions in `./gnugo_python/build_tools.py` to
download and compile gnu go in construction.

Make sure you have installed the compilation tools and dependencies required by gnugo,
buildtools.py will download and decompress the source code of gnugo in your project 
directory for compilation.

This tool will convert the `.a` files compiled by gnugo into `.so` files, 
allowing python to perform dynamic linking during execution.

# Usage
This repository provides several commonly used APIs, you can refer to the documentation of GNU GO, or related header files.
- [GNU Go Documentation: 17. Application Programmers Interface to GNU Go](https://www.gnu.org/software/gnugo/gnugo_17.html)

Note: The gnugo-3.8 version seems to have missing some APIs described in the docs,
Some APIs described in the docs may not appear in the symbols table, so it is recommended
to read the [gnugo source code](https://www.gnu.org/software/gnugo/download.html)
directly to understand the interface.

If your Go program crashes and you don’t know how to debug, I suggest you look at `./gnugo-3.8/interface/play_gtp.c`,
you can see how the gnugo gtp implementation calls api, maybe it will help.
In addition, we can track callstack through py-spy and gdb / lldb

Currently available two interfaces,
The interface provided by [pyclibrary](https://github.com/MatthieuDartiailh/pyclibrary) (`gnugo_python.gnugo`),
another [ctypes CDLL](https://docs.python.org/3/library/ctypes.html#ctypes.CDLL) interface (`gnugo_python.gnugo_fast`), the former will check the ABI,
in some cases, the performance will be poor, the latter can directly call the CDLL interface,
the disadvantage is that you need to pass the ABI yourself Structure 
(However, in the case of APIs using some basic data types, conversion may not be required,
But you may need to manually bind `argtypes` `restype`)

```python
from gnugo_python.gnugo_python.gnugo_python import *
from gnugo_python import gnugo, gnugo_fast
```

Currently, the two interfaces of CDLL are shared (but not guaranteed in the future),
so you can decide which interface to call according to your needs.

To get score, we can do it in two ways
```python
# manual binding, must assign restype
gnugo_fast.gnugo_estimate_score.restype = ctypes.c_float # only need to be set once
score = gnugo_fast.gnugo_estimate_score(None, None)

# pyclibrary calling
score, (upper, lower) = gnugo.gnugo_estimate_score()
print("upper bound {}, lower bound {}".format(upper, lower))
```

Also see [PyCLibrary documentation](https://pyclibrary.readthedocs.io/en/latest/get_started/basic_usage.html#calling-functions)

To play, We can check and make a move.
```python
from gnugo_python import *

player = WHITE
legal = gnugo_fast.is_legal(POS(x, y), player)
if legal:
    gnugo.play_move(POS(x, y), player)
```
Also see [Basic Data Structures in the Engine](https://www.gnu.org/software/gnugo/gnugo_17.html#SEC182)
 
gnugo native API
- `init_gnugo()`
- `gnugo_clear_board()`
- `is_legal()`

Also provides several functions to convert to the format used by gnugo
- `color2gnucolor()`

## ./engine/board.h
- `POS(x, y)`
- `I(pos)`
- `J(pos)`

## ./engine/gnugo.h
- `MIN_BOARD`
- `MAX_BOARD`

## Example

# Development
pytest

## Files 
- gnugo_python/
    - ./build_tools.py
    - ./gnugo_python.py
- ./tests
    - ./test_gnugo_python.py

## Roadmap
- board and go engine (`libboard.a`, `libengine.a`)
- `libsgf.a` It is not planned to support it now. The [existing python library](https://github.com/mattheww/sgfmill) is enough
to run 600k games in a reasonable time. For machine learning training data, pre-processing and saving should be enough.

This wrapper is mainly developed and tested on `*nix`. The low-level compiler
linker may cause the wrapper to be non available. It is not planned to support
windows. [WSL](https://learn.microsoft.com/en-us/windows/wsl/) can be used instead.

Pull requests are also welcome

# Evaluation

