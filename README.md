# GNU Go Python Binding

This repository provides [gnugo](https://www.gnu.org/software/gnugo/) API for python. Compared with using pure python, this binding makes the python Go application closer to the performance of native code.

In addition, python binding can avoid IPC overhead, such as using popen to wrap gnugo and communicate with python applications through [gtp protocol](https://www.gnu.org/software/gnugo/gnugo_19.html), 
Actually running two Processes, the middle process will reduce performance.

gnugo python binding can load the gnugo library into the same address space to improve performance.

Note:

This library is a third-party library and is not part of the official GNU GO.
The [Free Software Foundation](https://www.fsf.org/) owns the copyright of GNU GO.
Problems caused by using third-party libraries may not be officially supported,
and the use needs to be considered [Restrictions](https://www.gnu.org/software/gnugo/gnugo_1.html#SEC3)

# Dependency

for gnugo
```
ncurses
```

python3.10

# Build

# Usage
This repository provides several commonly used APIs, you can refer to the documentation of GNU GO, or related header files.
- [GNU Go Documentation: 17. Application Programmers Interface to GNU Go](https://www.gnu.org/software/gnugo/gnugo_17.html)

gnugo native API
- `init_gnugo()`
- `gnugo_clear_board()`
- `is_legal()`

Also provides several functions to convert to the format used by gnugo
- `color2gnucolor()`
- `xy2gnu_pos()`

Pull requests are also welcome
