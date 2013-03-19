======
Tabbed
======

This is modified tabbed extension for well known `urxvt-unicode
<http://software.schmorp.de/pkg/rxvt-unicode.html>`_ terminal emulator

Features
--------

* Possibility to add named tabs, through the X resources, which represents its
  *class*. Without any configuration only simple shell *class* is available
  under CTRL+SHIFT+N shortcut. After creating first custom shell this default
  will be discarded.

    .. image:: tabbed.png
        :alt: Named tabs

* Shortcuts can be attached to Super_L and Super_R keys together with others
* Fast navigation to first ten tabs (if available) with shortcut CTRL+[num],
  where *num* can be 0 to 9. CTRL+0 will switch to tenth tab.
* Numbers in tabs can be switched off in by setting
  resource:``URxvt.tabbed.tab-numbers: false``
* Integrated `activity indicator
  <http://mina86.com/2009/05/16/tabbed-urxvt-extension/>`_ with additional
  features like colors and different characters instead of simply asterisk
  depending on time.

    .. image:: tabbed.gif
        :alt: Indicator activity

* Integrated tab renaming from `stepb
  <http://github.com/stepb/urxvt-tabbedex>`_.  Default under SHIFT+UP, then type
  some text and RETURN for accept, ESC for cancel.

Installation
------------

Copy tabbed into ``~/.urxvt`` directory.

Add these to your ``~/.Xdefaults``::

    ! Perl extension config
    URxvt.perl-ext-common: default
    URxvt.perl-ext: tabbed
    ! Any scripts placed here will override global ones with the same name
    URxvt.perl-lib: /home/user/.urxvt/

    ! Tabbed extension configuration
    URxvt.tabbed.tabbar-fg: 8
    URxvt.tabbed.tabbar-bg: 0
    URxvt.tabbed.tab-fg:    15
    URxvt.tabbed.tab-bg:    8
    URxvt.tabbed.new-button: false

Creating custom classes
-----------------------

Let's assume, that one want to add three kind of custom shells:

* simple one (default shell in the system),
* midnight commander,
* root (namely - su command)

Three resources should be created in ``.Xdefaults``::

    URxvt.tabbed.tabcmds.1: N|shell
    URxvt.tabbed.tabcmds.2: R|root|su -
    URxvt.tabbed.tabcmds.3: M|mc|mc

Numbered attribute just after ``URxvt.tabbed.tabcmds`` resource is an ordinal
number, started from 1. There shouldn't be gaps between numbers, otherwise
custom shells defined after a gap will not work.

Resource values are pipe separated values, which are in order:

* **shortcut key**, which will be used for invoking custom shell together with
  *CTRL+SHIFT* keys. Mod4 (aka Super or Windows key) are not supported, and most
  probably will be removed from script soon, as lots of window managers out
  there make a big use of those keys.

*Note*: There is limitation for characters used as a shortcut. Because some of
them are used for control terminal itself (i.e. *CTRL+SHIFT+D* may not work),
and also other characters (digits, some special characters etc.).  Letters are
case insensitive.

* **name of the tab**, it could be anything but the pipe.
* **optional command**. If omitted, simple shell will be launched.

Startup tabs
------------

There is possibility to tell tabbed which tabs should be auto started during
first window launch. First, you'll need to have some custom tab commands.
Let's assume, that there are already defined three custom shells, like in
section above. If one wanted to start shell, mc and root session, following
line should be placed in ``~/.Xdefaults``::

    URxvt.session: N|M|R

Launching tabbed interface
--------------------------

After that invoke urxvt with perl extensions enabled::

    $ urxvt -pe tabbed

That's all.
