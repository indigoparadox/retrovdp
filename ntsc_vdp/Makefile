
# vim: ft=make noexpandtab

MAUG_DIR := ../../maug

SO_CFLAGS := -DRETROFLAT_OS_UNIX
SO_LDFLAGS := 

DLL_CFLAGS := -DRETROFLAT_OS_WIN

ifneq ("$(RELEASE)","RELEASE")
SO_CFLAGS += -DDEBUG_THRESHOLD=1 -DDEBUG_LOG -DDEBUG
DLL_CFLAGS += -DDEBUG_THRESHOLD=1 -DDEBUG_LOG -DDEBUG
endif

all: rvdpsdl1.so rvdpsdl2.so rvdpnt.dll

rvdpsdl1.so: retrovdp.c
	gcc -I$(MAUG_DIR)/src -I$(MAUG_DIR)/api/sdl \
		-fpic -shared -o $@ $< -DRETROFLAT_API_SDL1 \
		`pkg-config --libs --cflags sdl` $(SO_CFLAGS) $(SO_LDFLAGS)

rvdpsdl2.so: retrovdp.c
	gcc -I$(MAUG_DIR)/src -I$(MAUG_DIR)/api/sdl \
		-fpic -shared -o $@ $< -DRETROFLAT_API_SDL2 \
		`pkg-config --libs --cflags sdl2` $(SO_CFLAGS) $(SO_LDFLAGS)

rvdpnt.o: retrovdp.c
	wcc386 -I$(MAUG_DIR)/src -I$(MAUG_DIR)/api/win \
		-DRETROFLAT_API_WIN32 \
		-I$(WATCOM)/h/nt -bd $< -fo=$@ $(DLL_CFLAGS) -mf -5r -fp5

rvdpnt.dll: rvdpnt.o
	wlink system nt_dll name rvdpnt file rvdpnt

clean:
	rm -f rvdp*.so rvdp*.o rvdp*.dll

