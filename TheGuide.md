This is a work in progress.

Getting the .Net SDK installed:

1. Install git:   pkg install git
2. Clone the ports repo:   git clone https://git.freebsd.org/ports.git /usr/ports  (will take a bit)
4. CD to the .Net ports dir:  cd /usr/ports/lang/dotnet
5. Build/install:  make install clean (will take a bit)
   A. First you will see a long download
   B. You will then be presented with 2 dialog boxes in succession. Accept the defaults on both by hitting the Enter key.
   C. Wait through the long build process.
   D. You'll see a "gnugrp-3.11" dialog box.  Accept the defaults by hitting Enter.
   E. "krb5-____" dialog box will come up. Accept the defaults by hitting Enter.
   F. "gmake____" dialog box, Accept the defaults by hitting Enter.
   G. "m4____" dialog box, Accept the defaults by hitting Enter.
   H. "texinfo_____"  dialog box, Accept the defaults by hitting Enter.
   I. "help2man_____"  dialog box, Accept the defaults by hitting Enter.
   J. "p5-Locale-libintl__" dialog box, Accept the defaults by hitting Enter.
   K. "autoconf___"  dialog box, Accept the defaults by hitting Enter.
   L. "automake___" dialog box, Accept the defaults by hitting Enter.
   M. "node20__" dialog box, Accept the defaults by hitting Enter.
   N. "c-ares"
   O. "ninja"
   (at this point I'm wondering if I am doing things the "hard way", lol)
   
