SUBDIRS = ../.mingw bled ext2fs ms-sys syslinux/libfat syslinux/libinstaller syslinux/win libcdio/iso9660 libcdio/udf libcdio/driver ../res/loc
# As far as I can tell, the following libraries are *not* vulnerable to side-loading, so we link using their regular version:
NONVULNERABLE_LIBS = -lsetupapi -lole32 -lgdi32 -lshlwapi -lcrypt32 -lcomctl32 -luuid
# The following libraries are vulnerable (or have an unknown vulnerability status), so we link using our delay-loaded replacement:
# See https://github.com/pbatard/rufus/issues/2272
# Oh, and don't bother trying to delay load cfgmgr32.dll, even with the DECLSPEC_IMPORT __attribute__((visibility("hidden")))
# override. It just DOESN'T BLOODY WORK and you will waste HOURS on a wild goose chase!!!
VULNERABLE_LIBS = -ldwmapi-delaylib -lversion-delaylib -lvirtdisk-delaylib -lwininet-delaylib -lwintrust-delaylib

noinst_PROGRAMS = rufus

AM_V_WINDRES_0 = @echo "  RC     $@";$(WINDRES)
AM_V_WINDRES_1 = $(WINDRES)
AM_V_WINDRES_  = $(AM_V_WINDRES_$(AM_DEFAULT_VERBOSITY))
AM_V_WINDRES   = $(AM_V_WINDRES_$(V))

%_rc.o: %.rc ../res/loc/embedded.loc
	$(AM_V_WINDRES) $(AM_RCFLAGS) -i $< -o $@

rufus_SOURCES = badblocks.c dev.c dos.c dos_locale.c drive.c format.c format_ext.c format_fat32.c hash.c icon.c iso.c \
	localization.c net.c parser.c pki.c process.c re.c rufus.c smart.c stdfn.c stdio.c stdlg.c syslinux.c ui.c vhd.c wue.c
rufus_CFLAGS = -I$(srcdir)/ms-sys/inc -I$(srcdir)/syslinux/libfat -I$(srcdir)/syslinux/libinstaller -I$(srcdir)/syslinux/win -I$(srcdir)/libcdio -I$(srcdir)/../res $(AM_CFLAGS) \
	-DEXT2_FLAT_INCLUDES=0 -DSOLUTION=rufus
rufus_LDFLAGS = $(AM_LDFLAGS) -mwindows -L $(srcdir)/../.mingw
rufus_LDADD = rufus_rc.o bled/libbled.a ext2fs/libext2fs.a ms-sys/libmssys.a syslinux/libfat/libfat.a syslinux/libinstaller/libinstaller.a syslinux/win/libwin.a \
	libcdio/iso9660/libiso9660.a libcdio/udf/libudf.a libcdio/driver/libdriver.a $(NONVULNERABLE_LIBS) $(VULNERABLE_LIBS)
