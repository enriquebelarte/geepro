This code is completely based on original https://sourceforge.net/projects/geepro/ and this is just some correction to make it work in present distros.

I wanted some EPROM programmer software which would work on GNU/Linux for SPS200S v2.00 board and I found the above repo 
that seemed quite abandoned. Wine was not an option and this seemed to be at first sight the only working project with SPS200S.

Following its README instructions I got an error in the first `waf configure` step:

```
ValueError: invalid mode: 'rU'
```

I was using python 3.11 but running `waf` with python 2.7 solved the issue.

After that one I got a linker related error:


```bash
[101/101] cxxprogram: build_directory/src/buffer.cpp.1.o build_directory/src/chip.cpp.1.o build_directory/src/dummy.cpp.1.o build_directory/src/storings.cpp.1.o build_directory/src/iface.cpp.1.o build_directory/src/files.cpp.1.o build_directory/src/parport.cpp.1.o build_directory/src/timer.cpp.1.o build_directory/src/protocols.cpp.1.o build_directory/src/script.cpp.1.o build_directory/src/checksum.cpp.1.o build_directory/src/cfp.c.1.o build_directory/src/main.cpp.2.o -> build_directory/geepro
/usr/bin/ld: /home/enrique/Proyectos/geepro_0.0.4/geepro/build_directory/gui-gtk/libgui-gtk.a(bineditor.c.1.o):(.bss+0x0): multiple definition of `GuiBineditorColorsSet'; /home/enrique/Proyectos/geepro_0.0.4/geepro/build_directory/gui-gtk/libgui-gtk.a(gui.c.1.o):(.bss+0x0): first defined here
```

Could fix it changing the definition of `GuiBineditorColorsSet` at ```gui-gtk/bineditor.h``` as `typedef enum` instead of just `enum`.

After that everything built smoothly just by running :

```bash
python2 waf configure --prefix=/usr/local
python2 waf build
python2 waf install
```

Then ```geepro``` executable is available at ```/usr/local/bin```.

Hope it is useful for anyone looking for writing EPROMs with this board.
