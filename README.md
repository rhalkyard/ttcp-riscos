# TTCP port for ancient RISC OS

Here is a low-effort RISC OS port of the ancient `ttcp` TCP and UDP performance
test tool, that builds and runs on similarly ancient RISC OS releases (such as
3.10). Kind of like `iperf` (and in fact, a compatible precursor to it), but
with a lot less code to port.

Buildable with [Acorn C/C++ 5](https://www.4corn.co.uk/archive/acornc5/). Uses
Acorn's `InetLib`, `SocketLib` and `UnixLib` - a copy of these and their
headers, [snarfed from a mirror of Acorn's FTP
site](https://www.4corn.co.uk/aview.php?sPath=/acornftp/riscos/releases/networking/tcpip),
is included.

Changes made in support of RISC OS:

- Bring our own `getopt()` implementation (from musl libc) and `getrusage()`
  (barely functional hack) since RISC OS doesn't provide such niceties.
- Make an effort to close sockets, since they don't get closed for us at exit
  and end up hanging around and being a nuisance.
- Various fixes to get rid of compiler warnings (printf format specifiers, etc).
- Invert the sense of `-s` because that's how I prefer it (add `-s` to transmit
  from stdin and receive to stdout, no `-s` means transmit an internally
  generated pattern and discard received data)
- Rearrange source to Acorn C/C++'s liking

Various features are probably broken, and some of its performance counters are
clearly bogus (RISC OS has no contept of system vs. user time, or CPU time vs.
wall time), but it functions well enough for my purposes.
