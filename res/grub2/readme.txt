This directory contains the Grub 2.0 boot records that are used by Rufus

* Because of issue #2233, boot.img and core.img were created from the GRUB git
  source (@e67a551a48192a04ab705fca832d82f850162b64) obtained on 2023.04.25 from:
    https://git.savannah.gnu.org/cgit/grub.git/tree/?id=e67a551a48192a04ab705fca832d82f850162b64
  on a Debian 11.6 x64 system using the commands:
    ./autogen.sh
    # --enable-boot-time for Manjaro Linux
    ./configure --disable-nls --enable-boot-time
    make -j4
    cd grub-core
    ../grub-mkimage -v -O i386-pc -d. -p\(hd0,msdos1\)/boot/grub biosdisk fat exfat ext2 ntfs ntfscomp part_msdos -o core.img

* boot.img has been modified to nop the jump @ 0x66 as per grub2's setup.c comments:
  /* If DEST_DRIVE is a hard disk, enable the workaround, which is
     for buggy BIOSes which don't pass boot drive correctly. Instead,
     they pass 0x00 or 0x01 even when booted from 0x80.  */

* Note that, for convenience reasons, the content of boot.img is *not* the one that
  Rufus processes when writing the MBR.
  Instead, the byte array from src/ms-sys/inc/mbr_grub2.h (whose content is identical)
  is what Rufus uses. If you modify these files, be mindful that you may also need
  to update the array in mbr_grub2.h.

* For details, see src/format.c, src/msys/br.c and src/msys/inc/mbr_grub2.h.