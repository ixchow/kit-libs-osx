# kit-libs-osx

Statically-build libraries for games (OSX version).

Includes:
- SDL (a rather stripped-down build)
- glm
- libpng + zlib

All built using `build.py`.

## Using

Clone this directory as a subdirectory of whatever you are building.
Then add compiler/linker arguments as follows:
```
KIT_LIBS = kit-libs-osx

#for zlib:
C++FLAGS += -I$(KIT_LIBS)/zlib/include
LINKLIBS += -L$(KIT_LIBS)/zlib/lib -lz

#for libpng: (note: requires zlib LINKLIBS)
C++FLAGS += -I$(KIT_LIBS)/libpng/include
LINKLIBS += -L$(KIT_LIBS)/libpng/lib -lpng

#for SDL2:
C++FLAGS += `$(KIT_LIBS)/SDL2/bin/sdl2-config --cflags` -framework OpenGL
LINKLIBS += `$(KIT_LIBS)/SDL2/bin/sdl2-config --static-libs` -framework OpenGL

#for glm:
C++FLAGS += -I$(KIT_LIBS)/glm/include
```

(Note: this is Jam-like syntax; for Make, the variables are `CXXFLAGS` and `LDLIBS`.)

## Notes/Caveats

The `build.py` script hardcodes all library versions except glm, for which it fetches the latest version from git.

If you are maintaining a project that you want to build against local libraries only if they exist, you can simply place the `-I` and `-L` paths for kit-libs first.
For SDL you need to be slightly more clever, and modify the `PATH` environment variable in your back-quoted config calls:
```
`PATH=$(KIT_LIBS)/SDL2/bin:$PATH sdl2-config --cflags`
```
