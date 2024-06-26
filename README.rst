=========
Tabbedalt
=========

This is modified, alternative tabbed extension for well known `urxvt-unicode`_
terminal emulator

Features
--------

* Possibility to add named tabs, through the X resources, which run specified
  command or another shell (zsh/fish/ash/csh/ksh…). Without any configuration
  only default shell is available under default (``Shift+Down``) shortcut.

    .. image:: /screens/tabbed.png
        :alt: Named tabs

* Fast navigation to first ten tabs (if available) with shortcut ``Ctrl+[num]``,
  where *num* can be 0 to 9. ``CTRL+0`` will switch to tenth tab.
* Numbers in tabs can be switched off in by setting
  resource:``URxvt.tabbedalt.tab-numbers: false``
* Integrated `activity indicator`_ with additional features like colors and
  different characters instead of simply asterisk depending on time.

    .. image:: /screens/tabbed.gif
        :alt: Indicator activity

* Integrated tab renaming from `stepb`_.  Default under ``Shift+UP``, then type
  some text and press ``RETURN`` for accept or ``ESC`` for cancel.
* Added ability to configure own shortcuts using standard ``keysym`` urxvt
  option. See below for examples.
* Autohide tab when there is only one tab at the moment.
* Ability to assign hotkey to jump to last tab.
* Configurable confirm dialog for closing urxvt window, when there is more than
  one tab opened or there is an process still running.

Installation
------------

Copy tabbedalt into ``~/.urxvt/ext`` directory.

Add these to your ``~/.Xdefaults``::

    ! Perl extension config
    URxvt.perl-ext: tabbedalt

And that's it. On some systems, there might be a need to reload X resources:

.. code:: shell-session

   $ xrdb ~/.Xdefaults

For quickinstall and run urxvt, tabbedalt can be installed using following
command:

.. code:: shell-session

   $ curl --create-dirs https://raw.githubusercontent.com/gryf/tabbedalt/master/tabbedalt -o ~/.urxvt/ext/tabbedalt
   $ urxvt -pe tabbedalt

or even without "installing" at all:

.. code:: shell-session

   $ curl https://raw.githubusercontent.com/gryf/tabbedalt/master/tabbedalt -o /tmp/tabbedalt
   $ URXVT_PERL_LIB=/tmp urxvt -pe tabbedalt

Configure
---------

There are several additional things you can set contrary to the original tabbed
extension.

New button
~~~~~~~~~~

You can disable ``[NEW]`` button, just to save the space. Just add following
line to yours ``~/.Xdefaults`` file::

    URxvt.tabbedalt.new-button: false

Colors
~~~~~~

You can change all of the colors regarding tabs appearance. Here are defaults::

   URxvt.tabbedalt.tabbar-fg: 15
   URxvt.tabbedalt.tabbar-bg: 8
   URxvt.tabbedalt.tab-fg: 8
   URxvt.tabbedalt.tab-bg: 0
   URxvt.tabbedalt.active-fg: 1
   URxvt.tabbedalt.actives-fg: 5
   URxvt.tabbedalt.actived-fg: 4

Tab activity
~~~~~~~~~~~~

Tabs can change colors depending on the activity of terminal under certain tab.
Colors can be defined as described in section above. You can change the time
for either *group* of activity::

   URxvt.tabbedalt.tabbar-timeouts: 16:.:8:::4:+

The value can should be read as colon separated fields. In this case it would
be read as:

- 16 with symbol ``.``
- 8 with symbol ``:``
- 4 with symbol ``+``

Numeric values means amount of seconds, on which three group of activity will
be triggered. Those group are:

- Inactive for at least 16 seconds
- Inactive for at least 8 seconds
- Inactive for at least 4 seconds

Activity of the tab is always represented by asterisk sign (``*``).

You can change those values but bear in mind, that first group should have
timeout in seconds set higher, than middle one. You can also change symbols for
those groups.

Tab activity can be disabled by setting::

    URxvt.tabbedalt.disable-activity: true

Flickering
~~~~~~~~~~

If you happen to see fonts flickering on the terminal, you might want to set
this resource to true::

   URxvt.tabbedalt.stop-flickering: true

It is false by default, and it will affect how refreshing of the tabs and
windows is done. I was experienced it mostly on Intel graphics, with bitmap
fonts, but your mileage may vary.

Tab numbers
~~~~~~~~~~~

You can turn off tab numbers and leave only name of the tab. Handy to save the
space::

   URxvt.tabbedalt.tab-numbers: false

Autohide
~~~~~~~~

By default tab bar would be visible even with only one tab. To hide tab bar,
when there is a single tab, the following resource need to be set to true::

    URxvt.tabbedalt.autohide: true

Actions
~~~~~~~

There are several actions, which tabbedalt supports:

* ``new_tab`` - for tab creation
* ``rename_tab`` - for tab title renaming
* ``prev_tab`` - for jumping to previous tab
* ``next_tab`` - for jumping to next tab
* ``move_tab_left`` - for moving tab to the left
* ``move_tab_right`` - for moving tab to the right
* ``jump_to_tab`` - for quickly jump into specific tab

See next sections for examples. This feature was adapted from `tabbedex`_.

Disable default keystrokes
~~~~~~~~~~~~~~~~~~~~~~~~~~

By setting::

    URxvt.tabbedalt.disable-default-keys: true

you can completely remove default keystrokes for creating and navigating tabs.
In fact, if this resource is set to false (default), than tabbedalt will create
several keysyms mapped to the actions:

* ``Shift-Down``: ``new_tab`` - create tab
* ``Shift-Up``: ``rename_tab`` - for tab title renaming
* ``Shift-Left``: ``prev_tab`` - for jumping to previous tab
* ``Shift-Right``: ``next_tab`` - for jumping to next tab
* ``Shift-Left``: ``move_tab_left`` - for moving tab to the left
* ``Control-Right``: ``move_tab_right`` - for moving tab to the right
* ``Control-1..0``: ``jump_to_tab`` - for quickly jumping into first tenth tabs

It might be wise to define own shortcuts before disabling default keys.

#. ``new_tab``

   This action can have up to two arguments separated by colon, in a form::

       URxvt.keysym.desired-keys: tabbedalt:new_tab:tab-title:tab-command

   In this case for ``desired-keys`` shortcut there would be new tab created
   with title set to ``tab-title``, and command which tab run as
   ``tab-command``, for example::

       URxvt.keysym.Control-t: tabbedalt:new_tab:htop:htop

   where pressing control+t it will run new tab with title ``htop`` and command
   ``htop``.

   Both title and command may be omitted. If so, default title ``shell`` will
   be used in absence of title, and default shell will be run on missing
   command.

#. ``jump_to_tab``

   In this action, there is only one argument expected - number of the tab,
   i.e.::

       URxvt.keysym.Control-1: tabbedalt:jump_to_tab:1
       URxvt.keysym.Control-2: tabbedalt:jump_to_tab:2
       …
       URxvt.keysym.Control-0: tabbedalt:jump_to_tab:0

   Note, that tabs are indexed from 1, and tab 10th is numbered as 0.

#. The rest

   Al the rest of the actions (moving, jumping and renaming) are without
   argument. For example renaming will look like this::

       URxvt.keysym.Control-r: tabbedalt:rename_tab

Jump to last tab
~~~~~~~~~~~~~~~~

There is a possibility to tell tabbedalt to use ``jump_to_tab`` action to jump
to the last (rightmost) tab, instead of 10th. It can be done by setting
resource::

   URxvt.tabbedalt.zero-jump-last: true

so whatever keysym is assigned to ``tabbedalt:jump_to_tab:0`` will select last
tab, regardless if current number of tabs is more or less than 10. There is
still a way for selecting 10th tab, i.e.::

   URxvt.tabbedalt.zero-jump-last: true

   URxvt.keysym.Control-F1: tabbedalt:jump_to_tab:1
   URxvt.keysym.Control-F2: tabbedalt:jump_to_tab:2
   …
   URxvt.keysym.Control-F10: tabbedalt:jump_to_tab:10
   URxvt.keysym.Control-F11: tabbedalt:jump_to_tab:11
   URxvt.keysym.Control-F12: tabbedalt:jump_to_tab:12
   URxvt.keysym.Control-0: tabbedalt:jump_to_tab:0

In the example above, there are mapping for jump to tabs 1 - 12 using function
keys, and `Control+0` to jump whatever last tab is.

Confirm closing window
~~~~~~~~~~~~~~~~~~~~~~

When working with tabs, sometimes user accidentally could close the window, and
loose all the applications run on the tabs. There might be multiple tabs open,
or just one with running process on it (i.e. some editor), where closing window
by accident could result in data loss. To prevent this, there are two
additional resources that can be set. First one, disabled by default is::

    URxvt.tabbedalt.confirm-quit: false

When set to ``true`` it will either execute a message program or will display 
an urxvt overlay with the dialog directly on current tab. Note that overlay
dialog will expect the user to either press:

- ``y`` or ``enter`` key to close the window
- ``n`` or ``escape`` key to deny closing it

Second one is to provide X dialog program::

    URxvt.tabbedalt.confirm-program:

It might be whatever X program, which can accept text as an argument, and can
provide dialog which:

- have two buttons (i.e. yes/no, ok/cancel) where first will exit dialog with 0
  exit code and the latter will exit with whatever other number,
- destroying the dialog also emit exit code higher than 0.

So, for example standard `xmessage`_ can be used::

    URxvt.tabbedalt.confirm-program: xmessage -buttons ok:0,cancel:1

or `zenity`_::

    URxvt.tabbedalt.confirm-program: zenity --question --title 'Close window' --text

or `kdialog`_::

    URxvt.tabbedalt.confirm-program: kdialog --title 'Close window' --yesno

or… any other dialog programs which fulfill the above criteria.

Creating specific commands/shells
---------------------------------

Let's assume, that one want to add three kind of custom shells:

* simple one (default shell in the system),
* midnight commander,
* root (namely - su command)

A way to do this is to associate keystroke for it in ``.Xdefaults`` using
urxvts ``keysym`` option, and the actions described above::

    URxvt.keysym.Control-Shift-N: tabbedalt:new_tab:shell
    URxvt.keysym.Control-Shift-R: tabbedalt:new_tab:root:su -
    URxvt.keysym.Control-Shift-M: tabbedalt:new_tab:mc:mc

Resource values are colon separated values, which are in order:

* **plugin name:command**, which in this case of creating new tab will be
  ``tabbedalt:new_tab``.
* **title of the tab**, it could be anything but the colon.
* **optional command**. If omitted, default shell will be launched.

Renaming tabs
-------------

On runtime, tabs can be renamed using (by default) ``Shift+Up`` - now you can
type name for the tab. ``Return`` accept change, ``ESC`` cancels it. This
feature was taken from `stepb`_ tabbedx repository.

.. _urxvt-unicode: http://software.schmorp.de/pkg/rxvt-unicode.html
.. _activity indicator: http://mina86.com/2009/05/16/tabbed-urxvt-extension/
.. _stepb: http://github.com/stepb/urxvt-tabbedex
.. _tabbedex: https://github.com/mina86/urxvt-tabbedex
.. _xmessage: https://gitlab.freedesktop.org/xorg/app/xmessage
.. _zenity: https://wiki.gnome.org/Projects/Zenity
.. _kdialog: https://develop.kde.org/docs/administration/kdialog
