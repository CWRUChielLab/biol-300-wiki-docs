Customize the Wiki
================================================================================

1.  Start the virtual machine and log in::

        ssh -p 8015 hjc@dynamicshjc.case.edu

2.  Check for and install system updates on the virtual machine::

        sudo apt-get update
        sudo apt-get dist-upgrade
        sudo apt-get autoremove

3.  Change the name of the front page of the wiki. Move the existing `Main Page
    <https://dynamicshjc.case.edu:8014/wiki/Main_Page?redirect=no>`__
    to *Course syllabus* (capital "C", lowercase "s") and check "Leave a
    redirect behind" (the Move button is located near the top-right of the page,
    next to "View history").

    Next, delete the moved page (now called *Course syllabus*) so that last
    year's version of the page will import properly.
    
    Finally, edit the following page to make clicking on the wiki logo direct
    the user to the right place. Replace its contents with the following:

    - `MediaWiki:Mainpage
      <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:Mainpage>`__
      ::

        Course syllabus

4.  Modify the navigation sidebar. Edit the following page, and replace its
    contents with the following:

    - `MediaWiki:Sidebar
      <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:Sidebar>`__
      ::

        * navigation
        ** Course syllabus|Course syllabus
        ** Course policies|Course policies
        ** Special:Grades|Course grades
        ** Student list|Student list
        ** recentchanges-url|recentchanges
        ** randompage-url|randompage
        ** Help:Editing|help
        * SEARCH
        * TOOLBOX
        * LANGUAGES

5.  Add a reminder for students to the file upload form to include CWRU IDs in
    filenames. Edit the following page, and append the following:

    - `MediaWiki:Uploadtext
      <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:Uploadtext>`__
      ::

        <font color="red">'''''Please prefix all your filenames with your CWRU ID!'''''</font>

6.  Add a reminder for students to the WikiEditor dialog box to include CWRU IDs
    and file extensions in filenames. Edit the following page, and replace its
    contents with the following:

    - `MediaWiki:Wikieditor-toolbar-file-target
      <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:Wikieditor-toolbar-file-target>`__
      ::
    
        Filename: (PLEASE INCLUDE YOUR CWRU ID AND THE FILE EXTENSION!)

    .. todo::

        Add a similar message about prefixing uploaded file names with a
        student's user name to VisualEditor.

7.  Add custom styling. Edit the following page, and append the following:

    - `MediaWiki:Common.css
      <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:Common.css>`__

      .. code-block:: css

        /**********************************************
        *         CUSTOM BIOL 300 WIKI STYLES         *
        **********************************************/

        table.course-info {
            float: right;
            border: 1px solid #aaa;
            width:300px;
            text-align: center;
            border-collapse: collapse;
        }
        table.course-info caption {
            background: #2c659c;
            color: #fff;
            font-weight: bold;
        }
        table.course-info th {
            background: #dcdcdc;
        }
        div.course-example {
            background: #f9f9f9;
            border: 1px solid #ddd;
            padding: 1em;
            margin-top: 8px;
        }

8.  Create the term paper benchmark template. Edit the following page, and fill
    it with the following:

    - `Template:Termpaper
      <https://dynamicshjc.case.edu:8014/wiki/Template:Termpaper>`__
      ::

        '''{{#if: {{{1|}}} | [[User:{{{1}}}]]'s | My}} Term Paper Benchmarks'''

        * [[{{#if: {{{1|}}} | User:{{{1}}}}}/Term Paper Proposal | Term Paper Proposal]]. Due ? at 12:45 PM.

        * [[{{#if: {{{1|}}} | User:{{{1}}}}}/Model Plan | Model Plan]]. Due ? at 12:45 PM.

        * [[{{#if: {{{1|}}} | User:{{{1}}}}}/Benchmark I: Introduction | Benchmark I: Introduction]]. Due ? at 12:45 PM.

        * [[{{#if: {{{1|}}} | User:{{{1}}}}}/Benchmark II: Model Description | Benchmark II: Model Description]]. Due ? at 12:45 PM.

        * [[{{#if: {{{1|}}} | User:{{{1}}}}}/Benchmark III: Results | Benchmark III: Results]]. Due ? at 12:45 PM.

        * [[{{#if: {{{1|}}} | User:{{{1}}}}}/Benchmark IV: Discussion | Benchmark IV: Discussion]]. Due ? at 12:45 PM.

        * [[{{#if: {{{1|}}} | User:{{{1}}}}}/Final Term Paper | Final Term Paper]]. Due ? at 5 PM.
        <noinclude>
        <hr>
        '''Arguments'''
        # ''Username'' (optional): Links are created as subpages to this user's page. Defaults to using the current page as parent to linked subpages if omitted. Also replaces "My Term Paper Benchmarks" with "<nowiki>[[User:<username>]]</nowiki>'s Term Paper Benchmarks".
        </noinclude>

9.  Create a template for student user pages. Edit the following page, and fill
    it with the following:

    - `MediaWiki:NewArticleTemplate/User
      <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:NewArticleTemplate/User>`__
      ::

        {{termpaper}}

        <!--
            ATTENTION: DO NOT MAKE ANY CHANGES TO THIS PAGE!
            PRESS THE SAVE BUTTON BELOW, AND YOUR PERSONAL TERM
            PAPER TEMPLATE WILL BE CREATED. YOU SHOULD USE THE
            LINKS THAT APPEAR THERE FOR SAVING YOUR WORK.
        -->

    .. todo::

        Consider what should be done about the User namespace template if I
        can't get VisualEditor to work with NewArticleTemplate.

10. Create a blank template for term paper benchmarks. Create the page
    `MediaWiki:NewArticleTemplate/User/Subpage
    <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:NewArticleTemplate/User/Subpage>`__
    and leave it blank (you will first need to place some initial content on the
    page and save for it to be created, and then delete that content and save
    again).

11. Create templates for instructor and student comments. Edit the following
    page, and fill it with the following:

    - `MediaWiki:NewArticleTemplate/User Talk
      <https://dynamicshjc.case.edu:8014/wiki/MediaWiki:NewArticleTemplate/User_Talk>`__
      ::

        == Student Comments ==

        == Instructor Comments ==

12. Hide the "Category: Articles using small message boxes" message that appears
    at the bottom of some pages. Edit the following page and fill it with the
    following:

    - `Category:Articles using small message boxes <https://dynamicshjc.case.edu:8014/wiki/Category:Articles_using_small_message_boxes>`__
      ::

        __HIDDENCAT__

13. Shut down the virtual machine::

        sudo shutdown -h now

14. Using VirtualBox, take a snapshot of the current state of the virtual
    machine. Name it "**Wiki customization complete**".
