wpm — measure and improve your typing speed
===========================================

``wpm`` is a curses-based UNIX terminal program for measuring and improving your typing
speed (measured in words per minute, or WPM).

It depends only on standard Python libraries and therefore works with Python 2,
3 and PyPy.

Personal features
-----------------
- ``tab`` key resets tests
- edit quote collection to be generated tests 
- function to generate tests based on 500 most common english words
- new test ever test reset

Features
--------

- Over 4900 quotes in the database, shamelessly stolen from typeracerdata.com
- Extremely low typing latency!
- Timer starts when you strike the first key
- Completed text is *darkened*, helping you to focus ahead
- Keep separate scores for, e.g. type of keyboard, layout etc.
- Saves race scores in a CSV file that is a superset of TypeRacer's export
  format. Loads fine in Excel as well.
- Launches quickly in your terminal window for "in-between moments"

How to install
--------------

The recommended way is to install via PyPi

.. code:: bash

    $ pip install wpm

The above usually requires ``sudo``. If you don't want to install it
system-wide, you can use ``pip install wpm --user``.

Remember to check for upgrades with ``pip install --upgrade wpm``. You can also
install it from the source repository with

.. code:: bash

    $ pip install . [--user]

To just test the app without installing, type ``make run``.

How to run
----------

Just type ``wpm`` to start the program. The timer will start when you press the
first key. At any time, you can hit ``TAB`` to reset the test with a new test.

If you wnat to exit the application entirely feel free to ``esc``

You can backspace for the current word you're editing, if you make a mistake.
Mistakes will lower the accuracy score.

If you have problems finding the ``wpm`` file, you can also start it by typing
``python -m wpm``. You can also see options with ``python -m wpm --help``.

Calculating WPM
---------------

The WPM is calculated by dividing characters per second by five and then
multiplying that with 60. This is a well-known formula, but gives slightly
higher scores than on sites like typeracer.com. It is, however, good enough to
gauge your typing speed. And it works offline, and with your own texts.

Regarding TypeRacer, I really suggest everyone check it out. I use this program
merely to warm up before heading over to typeracer.com, where you can race
against others.

Loading custom texts
--------------------

If you want to type a custom text, run

.. code:: bash

    $ wpm --load yourfile.txt

If you use ``--load``, the author will currently be empty, the title will be
the basename of the file. The text ID will be its inode, just to make them
somewhat unique, so your stats will work.

You can also bundle up several texts into a single JSON file, using ``wpm
--load-json yourfile.json``. It must have the following format:

.. code:: json

    [
      {
        "author": "Author Name",
        "title": "Title of Work",
        "text": "The text to type here ..."
        "id": 123,
      },
      ...
    ]

The ``id`` is an optional integer. If you leave it out, an increasing,
zero-based integer will be used.

Format of race history
----------------------

wpm will save scores in a CSV file in `~/.wpm.csv`. This file can be loaded
directly into Excel. It uses the same format as TypeRacer, with the addition of
a few extra columns at the end. That means is should be possible to use
existing TypeRacer score history tools with this file with minor modifications.

The column order is:

========== ======== =======================================================
Column     Datatype Explanation
---------- -------- -------------------------------------------------------
race       int      Race number, always increasing and tied to timestamp
wpm        float    The average WPM for that quote that single time
accuracy   float    From 0 to 1, where 1 means no mistakes
rank       int      Always 1
racers     int      Always 1
text_id    int      Item number of text in given database
timestamp  str      UTC timestamp in strptime format `%Y-%m-%d %H:%M:%S.%f`
database   str      Either "default" or the basename of the file used
tag        str      A user supplied tag for that score (e.g., keyboard)
========== ======== =======================================================

Should there be any problem saving or loading the score history, it will copy
the existing file into `~/.wpm.csv.backup` and create a new one.

Tagging races
-------------

If you use `--tag=...` to tag your scores, this will be used until you change
it. It is just a free text field that is saved along with each race result. It
is useful to compare how well you are typing in various situations.

For example, perhaps you want to check if you are typing faster (but perhaps
less accurate?) on different keyboards, or you are learning a new keyboard
layout like Dvorak or Colemak and then use the tags `--tag=qwerty` and
`--tag=dvorak`.  If you are learning to touch type, or type with more fingers,
you often start out slower than your normal speed. Tagging is a great way to
keep track of your progress.

By running `wpm --stats` (or just `-s`), you will see a table of statistics,
grouped by each tag. It shows things like the average over time, along with
confidence and prediction intervals. An item like `n-10` means "the last 10
games".

The ~/.wpmrc file
-----------------

The first time you start wpm, it writes a `.wpmrc` file to your home directory.
It contains user settings that you can change. They are given in the table
below.

============== =========================== ======= =============================================================================
Section        Name                        Default Description
-------------- --------------------------- ------- -----------------------------------------------------------------------------
curses         escdelay                         15 Time in ms to wait for follow-up key after ESC
curses         window_timeout                   20 Time in ms until giving up waiting for a keypress. If negative, wait forever.
wpm            confidence_level               0.95 The confidence level for WPM statistics
wpm            cpm                               0 If positive, report CPM in stats instead of WPM
wpm            tab_spaces                        1 Number of spaces to expand tabs to
wpm            wrap_width                       -1 If positive, wrap text at this width
xterm256colors                                     Color codes for 256-color terminals (foreground, background)
xtermcolors                                        Color codes for ordinary terminals (foreground, background)
============== =========================== ======= =============================================================================
