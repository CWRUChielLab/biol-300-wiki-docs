Create ScholasticGrading Assignments
================================================================================
The ScholasticGrading MediaWiki extension must be populated with all the
assignments for the entire semester.

1.  Start biol300_2017 and log in::

        ssh -p 8015 hjc@dynamicshjc.case.edu

2.  Check for and install system updates on the virtual machine::

        sudo apt-get update
        sudo apt-get dist-upgrade
        sudo apt-get autoremove

3.  Start biol300_2016 if it is not already running.

4.  If you haven't done so already, give yourself (and any other TAs) privileges
    to edit grades. Navigate to

        https://dynamicshjc.case.edu:8014/wiki/Special:UserRights

    and add each instructor to the "grader" group.

5.  Export the ScholasticGrading student groups, assignments, and
    assignment-group-membership tables from the previous year (remember to
    add the MySQL password from last Spring to the first command, and note that
    you will be prompted three times for last year's BIOL 300 Wiki password)::

        ssh hjc@biol300.case.edu "mysqldump --user=root --password=<BIOL 300 Wiki 2016 MySQL password> -c biol300_wiki scholasticgrading_group scholasticgrading_assignment scholasticgrading_groupassignment > ~/scholasticgrading_2016.sql"
        scp hjc@biol300.case.edu:scholasticgrading_2016.sql ~/
        ssh hjc@biol300.case.edu "rm ~/scholasticgrading_2016.sql"

    .. todo::

        For exporting content from the 2016 wiki, the commands above are fine,
        but they should be revised for the future by using the ``wikidb``
        database name.

6.  Import the ScholasticGrading tables (note that this will delete any groups
    or assignments you have created manually, which would appear on the `Manage
    groups
    <https://dynamicshjc.case.edu:8014/w/index.php?title=Special:Grades&action=groups>`__
    page and the `Manage assignments
    <https://dynamicshjc.case.edu:8014/w/index.php?title=Special:Grades&action=assignments>`__
    page).

    The first command (``sed``) will do a search-and-replace on the year to make
    updating the dates in the next step a little faster. ::

        sed -i 's/2016/2017/g' ~/scholasticgrading_2016.sql
        mysql -u root -p wikidb < ~/scholasticgrading_2016.sql
        rm ~/scholasticgrading_2016.sql

    Enter the <MySQL password> when prompted.

7.  Update the dates for each assignment. Navigate to the assignments management
    page,

        https://dynamicshjc.case.edu:8014/w/index.php?title=Special:Grades&action=assignments

    First, check that the point total (listed at the bottom of the page) is 100
    for the "All students" group.

    Next, for each assignment, determine the appropriate new date using the
    `syllabus <https://dynamicshjc.case.edu:8014/wiki/Course_syllabus>`__ and
    update it. Remember to "Apply changes" often so you don't lose work!

8.  To see what the assignment list looks like to a student, navigate to the
    groups management page,

        https://dynamicshjc.case.edu:8014/w/index.php?title=Special:Grades&action=groups

    Add yourself to the "All students" group and press "Apply changes". Next,
    navigate to the "View all user scores" page,

        https://dynamicshjc.case.edu:8014/w/index.php?title=Special:Grades&action=viewalluserscores

    You should see the complete list of assignments under your name, along with
    the racetrack. Look this over to see if everything looks correct. All the
    runners should be all the way on the left. Once you are done, remove
    yourself from the "All students" group.

9.  Shut down the virtual machine::

        sudo shutdown -h now

10. Using VirtualBox, take a snapshot of the current state of the virtual
    machine. Name it "**ScholasticGrading assignments created**".
