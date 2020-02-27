# Tea4CUPS: Tee for CUPS

Tea4CUPS resembles the *nix command 'tee', but is a CUPS backend.

Tea4CUPS behaves just like any other CUPS backend, but allows you to **modify print jobs' datas as they pass through it** and to transparently **send them to any number of outputs**: other CUPS backends, files or pipes.

This for example allows you to output the **same print job on several printers at the same time**, which is not possible with CUPS.

Another possibility would be for example to send the same document to a printer, a PDF generator, a Fax, and as an attachment to an email message, all of this by printing it a single time. Or to wake a network printer using wakeonlan before printing.

Tea4CUPS allows this by permitting you to easily plug your own commands into the very last stage of CUPS' filtering chain.

# Installation

## Using your distro's package manager

### Ubuntu, Debian
```$ sudo apt install tea4cups```

### Arch
You can use the AUR package, for example using yay:

```$ yay -S tea4cups-py3-git```

## Manual installation

  0 - Download the latest release from https://codeberg.org/dadosch/tea4cups/releases and extract it.

  1 - Copy the 'tea4cups' file into CUPS' backend directory and give them the right permissions,
      for example :

        $ cp tea4cups /usr/lib/cups/backend

        $ chown root.root /usr/lib/cups/backend/tea4cups
        $ chmod 700 /usr/lib/cups/backend/tea4cups

  2 - Restart CUPS in order for this new backend to be detected.
     
  3 - Either from CUPS' web interface, add new printers with
      'Tea4CUPS managed' in front of the device's name, or by directly modifying CUPS' printers.conf file, or with the lpadmin command line tool, just prepend each DeviceURI
      value with ''tea4cups://''.
      If you modified printers.conf directly instead of using lpadmin then don't forget to
      restart CUPS.

  4 - Copy tea4cups.conf into CUPS' configuration directory (where
      cupsd.conf resides) :

        $ cp tea4cups.conf /etc/cups/

  5 - Modify tea4cups.conf to suit your needs. Several examples
      are included.

  6 - There's no 6!

  
# Different options to launch commands
Tea4CUPS provides three ways to launch commands :

- filters: A filter can modify the input data **before they
            are sent to the printer or made available to
            the pre and post hooks** defined below.
            Currently a single filter can be defined
            per print queue.

- prehooks: these are guaranteed to be launched **before the
                print job is sent to the real printer**.
                Any prehook which exits -1 can cancel the
                print job. Any number of prehooks can be
                defined for a particular print queue.

- posthooks: these are guaranteed to be launched **after the
                print job has been sent to the real printer**,
                unless the job was previously cancelled by a
                prehook. Any number of posthooks can be
                defined for a particular print queue.

# Available environment variables
                      
To help your own commands, Tea4CUPS makes available as part of the
environment several variables which can be used from the
commands you use in pre and post hooks :

- ''TEAPRINTERNAME'': The print queue name.
- ''TEADIRECTORY'': Tea4CUPS output directory.
- ''TEADATAFILE'': Full name of Tea4CUPS work file (in $TEADIRECTORY).
- ''TEAJOBSIZE'': Job's size in bytes.
- ''TEAMD5SUM'': MD5 sum of the job's datas.
- ''TEACLIENTHOST'': Client's hostname or IP address.
- ''TEAJOBID'': Job's Id.
- ''TEAUSERNAME'': Name of the user who launched the print job.
- ''TEATITLE'': Job's title.
- ''TEACOPIES'': Number of copies requested.
- ''TEAOPTIONS'': Options of the print job.
- ''TEAINPUTFILE'': Print job's data file or empty when job read from stdin.
- ''TEABILLING'': Job's billing code (lp -o job-billing=SomeCode file.ps)
- ''TEACONTROLFILE'': Job's IPP message file (usually /var/spool/cups/c?????)
- ''TEASTATUS'': Original CUPS backend's exit code : ONLY AVAILABLE FROM posthooks, obviously.

**None of these environment variables is available to filters.**

NB : Tea4CUPS requires a version of Python >= 3


=============================================================================

# Troubleshooting :

  1 - Set "LogLevel debug2" and "MaxLogSize 0" in CUPS' cupsd.conf

  2 - Restart CUPS.

  3 - Set "debug: yes" in the [global] section of tea4cups.conf

  4 - Print something on a print queue managed
      by Tea4CUPS.

  5 - Look at CUPS' error_log file for diagnostic and error messages.

=============================================================================

CUPS (Common UNIX(r) Printing System) is trademark property of Easy Software
Products.

You can download CUPS from :

        http://www.cups.org

=============================================================================

Please report any problem to the bug tracker at https://codeberg.org/dadosch/tea4cups/issues


(c) 2005-2018 Jerome Alet <alet@librelogiciel.com>

(c) 2005 Peter Stuge <stuge-tea4cups@cdy.org>

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA.
