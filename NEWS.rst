.. -*- rst -*-

======
 NEWS
======

The history of Pikzie

1.0.1: 2011-12-18
=================

Just package fix.

1.0.0: 2011-12-18
=================

Our repository has been moved to "GitHub
<https://github.com/clear-code/pikzie>"_ from sourceforge.net.

Improvements
------------

  * Supported subprocess. [Improved by Tetsuya Morimoto]
  * Supported Python 3.2.
  * Dropped Python 2.5 or earlier support.
  * Changed an abbreviation character for ``re.DOTALL`` to
    ``s`` instead of ``d`` because ``re.S`` means ``re.DOTALL``.
  * Supported regular expression flags in ``--name`` and
    ``--test-case``. ``/.../FLAGS`` format is
    used. ``/.../i`` means case insensitive match.

Fixes
-----

  * Fixed a problem that the same test files are loaded twice.
    [Fixed by Hideo Hattori]
  * Fixed a problem that test data output is failed when
    data driven test is failed.
    [pikzie-users-ja:22] [Fixed by Tetsuya Morimoto]

Thanks
------

  * Hideo Hattori
  * Tetsuya Morimoto

0.9.7: 2010-05-24
=================

  * normalize test module name on test loading

0.9.6: 2010-05-20
=================

  * add pikzie.utils module
  * add assert_exists
  * add assert_not_exist
  * support deeply test script loading
  * suppress needless colorization
  * improve assert_raise_call to accept exception instance
  * support data driven test

0.9.5: 2009-07-23
=================

  * support omit
  * support Windows partially

0.9.4: 2009-05-29
=================

  * fix failure report failure on non-color mode
    [Reported by Hideo Hattori]
  * treat an environment that TERM environment variable is
    ended with "term-color" as colorizable environment

0.9.3: 2008-12-25
=================

  * fix wrong 'sorted' detection
  * improve message detection in assert_search_syslog_call

0.9.2: 2008-06-26
=================

  * improve diff output
  * support module based test

0.9.1: 2008-06-25
=================

  * priority mode is off by default
  * support Python 2.5

0.9.0: 2008-03-31
=================

  * added priority mode (--priority/--no-priority options)
  * clearly specified LGPLv3 or later

0.8.0: 2008-03-24
=================

  * added --xml-report option for reporting test result as XML
  * improved diff format
  * improved test result output

0.7.0: 2008-02-26
=================

  * supported colorlized traceback.
  * added assert_kernel_symbol.

0.6.0: 2008-02-25
=================

  * compressed successive notifications.

0.5.0: 2008-02-20
=================

  * added --color-scheme option.

0.4.0: 2008-02-18
=================

  * added assert_run_command
  * added assert_search_syslog_call
  * added assert_open_file
  * added assert_try_call
  * assert_call_raise -> assert_raise_call
  * assert_call_nothing_raised -> assert_nothing_raised_call
  * added pikzie.pretty_print module

0.3.0: 2008-02-14
=================

  * supported test metadata
  * enabled colorized output on screen automatically
  * added pend and notify.

0.2.0: 2008-01-31
=================

  * added assert_call_nothing_raised
  * added auto test runner
    (You can run your tests without test runner script)
  * accepted test target file list from command line arguments
  * added --name, --test-case options
    (You can run only your tests or test cases tests that
    you specified)
  * supported colorized output (added --color option)
  * supported verbose mode (added --verbose option)
  * supported Python 2.3

0.1.0: 2008-01-28
=================

  * Initial release on SF.net.
