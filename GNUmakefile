# q2admin.so linux makefile

# This builds in the native mode of the current OS by default.

# Note that you might need to install the 32-bit libc package
# if it isn't already installed on your platform.
# Examples:
# sudo apt-get install ia32-libs
# sudo apt-get install libc6-dev-i386
# On Ubuntu 16.x use sudo apt install libc6-dev-i386

#this nice line comes from the linux kernel makefile
ARCH := $(shell uname -m | sed -e s/i.86/i386/ -e s/sun4u/sparc64/ -e s/arm.*/arm/ -e s/sa110/arm/ -e s/alpha/axp/)

# On 64-bit OS use the command: setarch i386 make all
# to obtain the 32-bit binary DLL on 64-bit Linux.

CC = gcc -std=c99 -Wall

# uncomment for ARM64 cross-compilation
#ARCH=aarch64
#CC=aarch64-linux-gnu-gcc -std=c99 -Wall

CFLAGS_BASE :=-fPIC -DARCH="$(ARCH)" -D__$(ARCH)__ -DLINUX -DSTDC_HEADERS -I/usr/include
LDFLAGS =-ldl -lm -shared

# Msys2 on Windows for MinGW
ifeq ($(shell uname -o),Msys)
LDFLAGS += -lregex
endif

# flavors of Linux
ifeq ($(shell uname),Linux)
#SVNDEV := -D'SVN_REV="$(shell svnversion -n .)"'
#CFLAGS_BASE += $(SVNDEV)
CFLAGS_BASE += -DLINUX
LIBTOOL = ldd
endif

# OS X wants to be Linux and FreeBSD too.
ifeq ($(shell uname),Darwin)
#SVNDEV := -D'SVN_REV="$(shell svnversion -n .)"'
#CFLAGS_BASE += $(SVNDEV)
CFLAGS_BASE += -DLINUX
LIBTOOL = otool
endif

ifeq ($(ARCH),i386)
  CFLAGS_RELEASE := -m32 -O3 $(CFLAGS_BASE)
else
  CFLAGS_RELEASE := -O3 $(CFLAGS_BASE)
endif
CFLAGS_DEBUG := -g $(CFLAGS_BASE)

build_release:
	$(MAKE) game$(ARCH).so CFLAGS="$(CFLAGS_RELEASE)"

build_debug:
	$(MAKE) game$(ARCH).so CFLAGS="$(CFLAGS_DEBUG)"

.c.o:
	$(CC) $(CFLAGS) -o $@ -c $<

OUTFILES = g_main.o zb_spawn.o zb_vote.o zb_ban.o zb_cmd.o zb_flood.o \
	zb_init.o zb_log.o zb_lrcon.o zb_msgqueue.o zb_util.o zb_zbot.o \
	zb_zbotcheck.o zb_disable.o zb_checkvar.o

game$(ARCH).so: $(OUTFILES)
	$(CC) $(OUTFILES) $(LDFLAGS) -o game$(ARCH).so
#	$(LIBTOOL) -r $@

zip: game$(ARCH).so
	strip game$(ARCH).so
	zip -9 q2admin-game$(ARCH).zip game$(ARCH).so

clean:
	rm -f $(OUTFILES) game$(ARCH).so

all: build_release

# dependencies
g_main.o: g_main.c g_local.h q_shared.h game.h
regex.o: regex.c g_local.h q_shared.h game.h regex.h
zb_ban.o: zb_ban.c g_local.h q_shared.h game.h
zb_checkvar.o: zb_checkvar.c g_local.h q_shared.h game.h
zb_clib.o: zb_clib.c g_local.h q_shared.h game.h
zb_cmd.o: zb_cmd.c g_local.h q_shared.h game.h
zb_disable.o: zb_disable.c g_local.h q_shared.h game.h
zb_flood.o: zb_flood.c g_local.h q_shared.h game.h
zb_init.o: zb_init.c g_local.h q_shared.h game.h
zb_log.o: zb_log.c g_local.h q_shared.h game.h
zb_lrcon.o: zb_lrcon.c g_local.h q_shared.h game.h
zb_msgqueue.o: zb_msgqueue.c g_local.h q_shared.h game.h
zb_spawn.o: zb_spawn.c g_local.h q_shared.h game.h
zb_util.o: zb_util.c g_local.h q_shared.h game.h
zb_vote.o: zb_vote.c g_local.h q_shared.h game.h
zb_zbot.o: zb_zbot.c g_local.h q_shared.h game.h
zb_zbotcheck.o: zb_zbotcheck.c g_local.h q_shared.h game.h

.PHONY: all build_release build_debug clean
