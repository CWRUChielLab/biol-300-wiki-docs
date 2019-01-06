Copy Old Wiki Contents
================================================================================

1.  Start biol300_2017 and log in::

        ssh -p 8015 hjc@dynamicshjc.case.edu

2.  Check for and install system updates on the virtual machine::

        sudo apt-get update
        sudo apt-get dist-upgrade
        sudo apt-get autoremove

3.  Start the old (2016) virtual machine if it is not already running.

4.  Export the policy and unit pages from last year's wiki. You may have decided
    after last spring's semester ended to make some changes to these pages on
    either the live wiki (biol300.case.edu) or the development wiki
    (biol300dev.case.edu). To determine where the latest changes can be found,
    visit these pages to view the latest edits on each wiki:

    - `Live wiki recent changes <https://biol300.case.edu/w/index.php?title=Special:RecentChanges&days=500&from=&limit=500>`__
    - `Dev wiki recent changes <https://biol300dev.case.edu/w/index.php?title=Special:RecentChanges&days=500&from=&limit=500>`__

    If you made changes in parallel to both wikis that you want to keep, you may
    need to manually propogate some of those edits.

    After determining where you should export pages from, visit *Special:Export*
    on that wiki (`live wiki
    <https://biol300.case.edu/wiki/Special:Export>`__ | `dev wiki
    <https://biol300dev.case.edu/wiki/Special:Export>`__). Select
    these options,

    - Check "Include only the current revision, not the full history"
    - Do NOT check "Include templates"
    - Check "Save as file"

    and paste the names of the following pages into the text field:

    .. container:: collapsible

        BIOL 300 Wiki pages to export

        ::

            Course policies
                Class attendance
                Team evaluations
                Concepts and attitudes surveys
                Problem benchmarks
                Comments on other students' term paper benchmarks
                Term Paper Template
                Model Plan
                    Exemplary Model Plan
                Selecting a published model to reproduce
                Term paper proposal
                    Exemplary Term Paper Proposal 1
                    Exemplary Term Paper Proposal 2
                Benchmark I: Introduction
                    Exemplary Introduction Draft 1
                    Exemplary Introduction Draft 2
                    Exemplary Introduction Draft 3
                    Exemplary Introduction Draft 4
                    Exemplary Introduction Draft 5
                Benchmark II: Model Description
                    Exemplary Model Description Draft 1
                    Exemplary Model Description Draft 2
                    Exemplary Model Description Draft 3
                    Exemplary Model Description Draft 4
                    Exemplary Model Description Draft 5
                Benchmark III: Results
                    Exemplary Results Draft 1
                    Exemplary Results Draft 2
                    Exemplary Results Draft 3
                    Exemplary Results Draft 4
                    Exemplary Results Draft 5
                Benchmark IV: Discussion
                    Exemplary Discussion Draft 1
                    Exemplary Discussion Draft 2
                    Exemplary Discussion Draft 3
                    Exemplary Discussion Draft 4
                    Exemplary Discussion Draft 5
                Term paper
                    Exemplary Final Term Paper 1
                    Exemplary Final Term Paper 2
                    Exemplary Final Term Paper 3
                    Exemplary Final Term Paper 4
                    Exemplary Final Term Paper 5
                    Exemplary Final Term Paper 6
                Student Presentations
                    Presentation Guidelines
                Rationale for modeling and modeling tools
                Plagiarism
                Don't Cheat
                The rules apply to everyone, including you

            Course syllabus
                List of Discussion benchmarks
                List of final term papers
            Student list
            Help:Editing
            BIOL 300 Wiki:Copyrights
            Template:Instructor links
            User:Hjc
            User:Jpg18
            Sandbox
            Wiki Todo list
            Hjc Todo list

    Export the pages and save the XML file when given the option.

5.  On the 2017 virtual machine, visit `Special:Import
    <https://dynamicshjc.case.edu:8014/wiki/Special:Import>`__ and upload the
    XML file obtained from the 2016 wiki (choose "Import to default locations").

6.  Since it is possible that the list above is incomplete, visit
    `Special:WantedPages
    <https://dynamicshjc.case.edu:8014/w/index.php?title=Special:WantedPages&limit=500&offset=0>`__
    to determine which pages are still missing.

    There will be several missing pages related to the class that should be
    ignored. These are the pages begining with the slash ("/") character, such
    as */Model Plan*. These appear in the list because the *Template:Termpaper*
    page uses relative links for the term paper benchmanks.

    If necessary, repeat steps 4-5 until no relevant pages are missing.

7.  The following pages need to be updated with new dates, personnel, office
    hours times, etc., or out-dated contents need to be cleared:

    - `Course syllabus
      <https://dynamicshjc.case.edu:8014/wiki/Course_syllabus>`__
    - `Course policies
      <https://dynamicshjc.case.edu:8014/wiki/Course_policies>`__
    - `Student list
      <https://dynamicshjc.case.edu:8014/wiki/Student_list>`__
    - `Student Presentations
      <https://dynamicshjc.case.edu:8014/wiki/Student_Presentations>`__
    - `Template:Termpaper
      <https://dynamicshjc.case.edu:8014/wiki/Template:Termpaper>`__

8.  If you'd like to add or remove term paper benchmark exemplars, now is a good
    time to do so. If you remove any, be sure to also delete associated files
    and images from the "Files to Import" directory.

    .. todo::

        The "Files to Import" directory is now hosted online. Add instructions
        for modifying it.

9.  On the virtual machine, download and then import into the wiki a collection
    of images and files. This includes the wiki logo, favicon, and figures from
    benchmark exemplars::

        wget -P ~ https://dynamicshjc.case.edu/~vbox/biol300/_downloads/BIOL-300-Files-to-Import.tar.bz2
        tar -xjf ~/BIOL-300-Files-to-Import.tar.bz2 -C ~
        php /var/www/mediawiki/maintenance/importImages.php --user=Hjc ~/BIOL-300-Files-to-Import
        sudo apache2ctl restart
        rm -rf ~/BIOL-300-Files-to-Import*

    If you'd like to view the collection of files, you can download it to your
    personal machine here: :download:`BIOL-300-Files-to-Import.tar.bz2
    </_downloads/misc/BIOL-300-Files-to-Import.tar.bz2>`

10. .. todo::

        Update this step with instructions for adding files to the online
        "BIOL-300-Files-to-Import.tar.bz2" archive, and move the
        ``fetch_wiki_files.sh`` script to an external file in the docs source.

    Visit `Special:WantedFiles
    <https://dynamicshjc.case.edu:8014/w/index.php?title=Special:WantedFiles&limit=500&offset=0>`__
    to determine which files are still missing. Files on this list that are
    struckthrough are provided through `Wikimedia Commons
    <https://commons.wikimedia.org/wiki/Commons:Welcome>`__ and can be ignored.

    If there are only a few files missing, download them individually from the
    old wiki, add them to the "Files to Import" directory, and upload them
    manually.

    If there are many files missing (which is likely to happen if you added a
    new exemplar), you can use the following script to download them from the
    old wiki in a batch.

    On your personal machine, create the file ::

        vim fetch_wiki_files.sh

    and fill it with the following:

    .. container:: collapsible

        fetch_wiki_files.sh

        .. code-block:: bash

            #!/bin/bash

            # This script should be run with a single argument: the path to a file
            # containing the names of the files to be downloaded from the wiki,
            # each on its own line and written in the form "File:NAME.EXTENSION".
            INPUT="$1"
            if [ ! -e "$INPUT" ]; then
                echo "File \"$INPUT\" not found!"
                exit 1
            fi

            # MediaWiki provides an API for querying the server. We will use it
            # to determine the URLs for directly downloading each file.
            WIKIAPI=https://biol300.case.edu/w/api.php

            # The result of our MediaWiki API query will be provided in JSON and
            # will contain some unnecessary meta data. We will use this Python
            # script to parse the query result. It specifically extracts only the
            # URLs for directly downloading each file.
            SCRIPT="
            import sys, json

            data = json.loads(sys.stdin.read())['query']['pages']

            for page in data.values():
                if 'invalid' not in page and 'missing' not in page:
                    print page['imageinfo'][0]['url']
            "

            # Create the directory where downloaded files will be saved
            DIR=downloaded_wiki_files
            mkdir -p $DIR

            # While iterating through the input line-by-line...
            while read FILENAME; do
                if [ "$FILENAME" ]; then
                    echo -n "Downloading \"$FILENAME\" ... "

                    # ... query the server for a direct URL to the file ...
                    JSON=`curl -s -d "action=query&format=json&prop=imageinfo&iiprop=url&titles=$FILENAME" $WIKIAPI`

                    # ... parse the query result to obtain the naked URL ...
                    URL=`echo $JSON | python -c "$SCRIPT"`

                    if [ "$URL" ];
                    then
                        # ... download the file
                        cd $DIR
                        curl -s -O $URL
                        cd ..
                        echo "success!"
                    else
                        echo "not found!"
                    fi
                fi
            done < "$INPUT"

    Make the script executable::

        chmod u+x fetch_wiki_files.sh

    Copy the bulleted list of missing files found at `Special:WantedFiles
    <https://dynamicshjc.case.edu:8014/w/index.php?title=Special:WantedFiles&limit=500&offset=0>`__
    and paste them into this file::

        vim wanted_files_list.txt

    You can use this Vim command to clean up the list::

        :%s/^\s*File:\(.*\)\%u200f\%u200e (\d* link[s]*)$/File:\1/g

    Finally, execute the script to download all the files in the list::

        ./fetch_wiki_files.sh wanted_files_list.txt

    The downloaded files will be saved in the ``downloaded_wiki_files``
    directory. Copy these to the "Files to Import" directory and upload them to
    the new wiki manually or using the ``importImages.php`` script used in step
    9.

11. Protect every image and media file currently on the wiki from vandalism.
    Access the database::

        mysql -u root -p wikidb

    Enter the <MySQL password> when prompted. Execute these SQL commands (the
    magic number 6 refers to the `File namespace
    <https://www.mediawiki.org/wiki/Manual:Namespace_constants>`__):

    .. code-block:: sql

        INSERT IGNORE INTO page_restrictions (pr_page,pr_type,pr_level,pr_cascade,pr_expiry)
            SELECT p.page_id,'edit','sysop',0,'infinity' FROM page AS p WHERE p.page_namespace=6;
        INSERT IGNORE INTO page_restrictions (pr_page,pr_type,pr_level,pr_cascade,pr_expiry)
            SELECT p.page_id,'move','sysop',0,'infinity' FROM page AS p WHERE p.page_namespace=6;
        INSERT IGNORE INTO page_restrictions (pr_page,pr_type,pr_level,pr_cascade,pr_expiry)
            SELECT p.page_id,'upload','sysop',0,'infinity' FROM page AS p WHERE p.page_namespace=6;

    Type ``exit`` to quit.

    You do not need to protect any wiki pages because the Lockdown extension for
    MediaWiki does this for you!

12. Shut down the virtual machine::

        sudo shutdown -h now

13. Using VirtualBox, take a snapshot of the current state of the virtual
    machine. Name it "**2016 wiki contents migrated**".
