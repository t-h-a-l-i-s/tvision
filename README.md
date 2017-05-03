# tvision
Fork of Sergio Sigala's Turbo Vision Port, fixed to build modern autotools.

# Cygwin
If the sources had been checked out accidentially with dos file endings convert them first (set the core.autocrlf setting to false for this project):
```
find . -name '*' | xargs dos2unix
```
The ncurses library headers have to be installed. Additionally there were problems with the installed libtool version and the project finally build successfully with the 3 commands:
```
autoreconf --force --install
./configure
make
```
