# GNU Go Python Binding

This repository provides [gnugo](https://www.gnu.org/software/gnugo/) API for python. Compared with using pure python, this binding makes the python Go application closer to the performance of native code.

In addition, python binding can avoid IPC overhead, such as using popen to wrap gnugo and communicate with python applications through [gtp protocol](https://www.gnu.org/software/gnugo/gnugo_19.html), 
Actually running two Processes, the middle process will reduce performance.

gnugo python binding can load the gnugo library into the same address space to improve performance.

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

Also provides several functions to convert to the format used by gnugo
- `color2gnucolor()`
- `xy2gnu_pos`()

Pull requests are also welcome
