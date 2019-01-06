Configuring the Development Server
================================================================================

1.  If biol300_2017 is running, log in and shut it down::

        ssh hjc@biol300.case.edu
        sudo shutdown -h now

2.  If biol300dev_2016 is running, shut it down as well::

        ssh hjc@biol300dev.case.edu
        sudo shutdown -h now

3.  Using VirtualBox, take a snapshot of the current state of biol300_2017. Name
    it "**Last shared state between biol300 and biol300dev**".

4.  In VirtualBox, create a clone of biol300_2017 by selecting the virtual
    machine and Machine > Clone, and choosing the following settings:

    - New machine name
        - Name: biol300dev_2017
        - Do NOT check "Reinitialize the MAC address of all network cards"
    - Clone type
        - Full clone
    - Snapshots
        - Current machine state

    Drag-and-drop the new virtual machine into the "2017" group under "BIOL
    300".

5.  In VirtualBox, select biol300dev_2017 and choose Settings > Network.
    Verify that the "Attached to" setting is set to "Bridged Adapter". Under
    Advanced, change the MAC address to match BIOL300Dev's (see
    :ref:`mac-addresses-lookup` for instructions).

6.  Restart biol300_2017. Start biol300dev_2017 for the first time and log
    in::

        ssh-keygen -R biol300dev.case.edu
        ssh hjc@biol300dev.case.edu

7.  Change the name that the development server uses to identify itself to the
    network::

        sudo hostnamectl set-hostname biol300dev

8.  You should now be able to access it in a browser:

        https://biol300dev.case.edu

9.  .. todo::

        Make the backup script that lives on DynamicsHJC and the cron job list
        downloadable.

    Log into the vbox account on DynamicsHJC::

        ssh vbox@dynamicshjc.case.edu

    Create a script that remotely executes the backup script on the development
    virtual machine and moves the archive to the backup drive. Create the file
    ::

        mkdir -p /Volumes/CHIELWIKI/backups/biol300dev/2017
        vim /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh

    and fill it with the following:

    .. code-block:: bash

        #!/bin/bash

        REMOTESSH="ssh hjc@biol300dev.case.edu"
        #REMOTESSH="ssh -p 8015 hjc@dynamicshjc.case.edu"

        REMOTESCP="scp -q hjc@biol300dev.case.edu"
        #REMOTESCP="scp -q -P 8015 hjc@dynamicshjc.case.edu"

        REMOTEFILE=biol300dev-backup-`date +'%Y%m%dT%H%M%S%z'`.tar.bz2
        if [ -z "$1" ]; then
            LOCALFILE=/Volumes/CHIELWIKI/backups/biol300dev/2017/$REMOTEFILE
        else
            LOCALFILE=/Volumes/CHIELWIKI/backups/biol300dev/2017/$1
        fi

        $REMOTESSH backup-wiki -q backup $REMOTEFILE
        $REMOTESCP:$REMOTEFILE $LOCALFILE
        chmod go= $LOCALFILE
        $REMOTESSH rm $REMOTEFILE

10. Make the script executable::

        chmod u+x /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh

11. In the vbox account on DynamicsHJC, create a backup schedule. Create the
    file ::

        vim /Volumes/CHIELWIKI/backups/biol300dev/2017/crontab

    and fill it with the following:

    .. code-block:: bash

        ################################################################################
        #                                  BIOL300DEV                                  #
        ################################################################################
        # Make hourly backups on the odd-numbered hours (except 1 AM and 3 AM)
        0 5,9,13,17,21  * * * /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh hourA.tar.bz2
        0 7,11,15,19,23 * * * /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh hourB.tar.bz2
        # Make daily backups at 1 AM
        0 1 * * 0 /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh sunday.tar.bz2
        0 1 * * 1 /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh monday.tar.bz2
        0 1 * * 2 /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh tuesday.tar.bz2
        0 1 * * 3 /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh wednesday.tar.bz2
        0 1 * * 4 /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh thursday.tar.bz2
        0 1 * * 5 /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh friday.tar.bz2
        0 1 * * 6 /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh saturday.tar.bz2
        # Make weekly backups at 3 AM on the 1st, 8th, 15th, and 22nd of the month
        0 3 1  * * /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh `date +'\%Y-\%m'`.tar.bz2
        0 3 8  * * /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh 8th.tar.bz2
        0 3 15 * * /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh 15th.tar.bz2
        0 3 22 * * /Volumes/CHIELWIKI/backups/biol300dev/2017/backup-biol300dev.sh 22nd.tar.bz2

12. Schedule the backups. In the vbox account on DynamicsHJC, dump the existing
    scheduled jobs to a temporary file::

        crontab -l > /tmp/crontab.old

    Edit the temporary file, and delete the backup jobs for last year's
    BIOL300Dev. You can use ``Shift+v`` in Vim to enter Visual Line mode, the up
    and down arrow keys to select a block of lines, and ``d`` to delete them all
    at once. ::

        vim /tmp/crontab.old

    Now append the new jobs to the old and schedule them::

        cat {/tmp/crontab.old,/Volumes/CHIELWIKI/backups/biol300dev/2017/crontab} | crontab

    Verify that the backup jobs for this year's BIOL300Dev are properly
    scheduled, and that backup jobs for last year's BIOL300Dev are absent::

        crontab -l

13. Fix SSH authentication into BIOL300Dev from the vbox account. You will be
    asked to accept the unrecognized fingerprint of the virtual machine --- this
    is expected --- but you should NOT need to enter your password. The IP
    address is the static IP for biol300dev.case.edu, obtained using ``host
    biol300dev.case.edu``. ::

        ssh-keygen -R biol300dev.case.edu -R 129.22.139.48
        ssh hjc@biol300dev.case.edu

    If this works without you needing to enter a password, automatic
    authentication is properly configured. You should ``logout`` to return to
    the vbox account on DynamicsHJC, and ``logout`` again to return to the
    virtual machine.

14. Shut down the development virtual machine::

        sudo shutdown -h now

15. Using VirtualBox, take a snapshot of the current state of biol300dev_2017.
    Name it "**biol300dev configured**".

16. Restart biol300dev_2017.
