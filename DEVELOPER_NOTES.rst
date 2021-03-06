Sharing development boards
==========================

To avoid conflicts for development boards on the server, while using a board you must hold the corresponding lock file present in ``/run/board``. Holding the lock file grants you exclusive access to the board.

To lock the KC705 for 30 minutes or until Ctrl-C is pressed:
::
  flock --verbose /run/boards/kc705 sleep 1800
Check that the command acquires the lock, i.e. prints something such as:
::
  flock: getting lock took 0.000003 seconds
  flock: executing sleep

To lock the KC705 for the duration of the execution of a shell:
::
  flock /run/boards/kc705 bash

You may also use this script:
::
  #!/bin/bash
  exec flock /run/boards/$1 bash --rcfile <(cat ~/.bashrc; echo PS1=\"[$1\ lock]\ \$PS1\")

If the board is already locked by another user, the ``flock`` commands above will wait for the lock to be released.

To determine which user is locking a board, use:
::
  fuser -v /run/boards/kc705


Selecting a development board with artiq_flash
==============================================

::
  artiq_flash --preinit-command "ftdi_location 5:2"   # Sayma 1
  artiq_flash --preinit-command "ftdi_location 3:10"  # Sayma 2
  artiq_flash --preinit-command "ftdi_location 5:1"   # Sayma 3


Deleting git branches
=====================

Never use ``git push origin :branch`` nor ``git push origin --delete branch``, as this can delete code that others have pushed without warning. Instead, always delete branches using the GitHub web interface that lets you check better if the branch you are deleting has been fully merged.
