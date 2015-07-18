.. -*- rst -*-

========
 README
========

Name
====

Pikzie

Author
======

- Kouhei Sutou <kou@clear-code.com>

License
=======

LGPLv3 or later

What's this?
============

Pikzie is an easy to write/debug Unit Testing Framework for
Python.

Pikzie provides the following features that are lacked in
unittest.py included in the standard Python distribution:

- Pythonic API
- a lot of assertions
- outputs result with useful format for debugging.

And Pikzie has the following features:

- ...

Dependencies
============

- Python >= 2.6 (Python 3.x is also supported.)

Install
=======

Install with easy_install::

  % sudo easy_install Pikzie

Install with pip::

  % sudo pip install Pikzie

Install from tar.gz::

  % wget http://downloads.sourceforge.net/pikzie/pikzie-1.0.0.tar.gz
  % tar xvzf pikzie-1.0.0.tar.gz
  % cd pikzie-1.0.0
  % sudo python setup.py install

Repository
==========

There is "clear-code/pikzie
<https://github.com/clear-code/pikzie>"_ on GitHub.

git::

  % git clone https://github.com/clear-code/pikzie.git
  % cd pikzie
  % sudo python setup.py install

Usage
=====

We assume that you have the following directory structure::

  . -+- lib  --- your_module --- ...
     |
     +- test -+- run-test.py
              |
              +- __init__.py
              |
              +- test_module1.py
              |
              +- ...

Template
--------

Here is a template of a test case::

  import pikzie
  import test_target_module

  class TestYourModule(pikzie.TestCase):
      def setup(self):
          self.setup_called = True

      def teardown(self):
          self.setup_called = False

      def test_condition(self): # starts with "test_"
          self.assert_true(self.setup_called)

test/run-test.py is the following::

  #!/usr/bin/env python

  import sys
  import os

  base_dir = os.path.abspath(os.path.join(os.path.dirname(__file__), ".."))
  sys.path.insert(0, os.path.join(base_dir, "lib"))
  sys.path.insert(0, base_dir)

  import pikzie

  sys.exit(pikzie.Tester().run())

Make test/run-test.py executable::

  % chmod +x test/run-test.py

test/test_*.py are automatically loaded and defined tests
are ran by invoking run-test.py like the following::

  % test/run-test.py

You can pass zero or more options to test/run-test.py like the
following::

  % test/run-test.py --priority

You can see all available options by ``--help`` option like the
following::

  % test/run-test.py --help

See "Options" section in this document for more details.

Test result
===========

Here is an example test result::

  ....F..............................

  1) Failure: TestLoader.test_collect_test_cases: sorted(test_case_names))
  expected: <['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']>
   but was: <['TestXXX1', 'TestXXX2', 'TestYYY']>
  diff:
  - ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']
  ?                                   -----------

  + ['TestXXX1', 'TestXXX2', 'TestYYY']
  /home/kou/work/python/pikzie/test/test_loader.py:30: test_collect_test_cases(): sorted(test_case_names))

  Finished in 0.013 seconds

  35 test(s), 55 assertion(s), 1 failure(s), 0 error(s), 0 pending(s), 0 notification(s)

Progress
--------

A part that contains "." and "F" of the test result shows
test progress::

  ....F..............................

Each "." and "F" shows a test case (test function). "."
shows a test case that is succeeded and "F" shows a test
case that is failed. There are "E", "P" and "N". They shows
error, pending and notification respectively. Here is a
summary of test case marks:

.
  A succeeded test

F
  A failed test

E
  A test that had an error

P
  A test that is marked as pending

N
  A test that had an notification

The above marks are showed after each test is finished. We
can confirm the test progress from the output in testing.

Summary of test result
----------------------

Pikzie outputs a summary of test result after all tests are
finished. The first of a summary is a list of a detail of
test result of non-succeeded test. In the example, Pikzie
outputs a detail of test result because there is a failure::

  1) Failure: TestLoader.test_collect_test_cases: sorted(test_case_names))
  expected: <['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']>
   but was: <['TestXXX1', 'TestXXX2', 'TestYYY']>
  diff:
  - ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']
  ?                                   -----------

  + ['TestXXX1', 'TestXXX2', 'TestYYY']
  /home/kou/work/python/pikzie/test/test_loader.py:30: test_collect_test_cases(): sorted(test_case_names))

In the example, TestLoader.test_collect_test_cases test case
is failed and shows that we expected::

  ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']

but was::

  ['TestXXX1', 'TestXXX2', 'TestYYY']

The following part of "diff:" marks different parts to find
difference easily::

  diff:
  - ['TestXXX1', 'TestXXX2', 'TestYYY', 'TestZZZ']
  ?                                   -----------

  + ['TestXXX1', 'TestXXX2', 'TestYYY']

The failed assertion is in test_collect_test_cases() method
in /home/kou/work/python/pikzie/test/test_loader.py at 30th
line and the line's content is the following::

  sorted(test_case_names))

Elapsed time for testing is showed after a list of a detail
of test result::

  Finished in 0.013 seconds

The last line is an summary of test result::

  35 test(s), 55 assertion(s), 1 failure(s), 0 error(s), 0 pending(s), 0 notification(s)

Here are the means of each output:

n test(s)
  n test case(s) (test function(s)) are run.

n assertion(s)
  n assertion(s) are passed.

n failure(s)
  n assertion(s) are failed.

n error(s)
  n error(s) are occurred (n exception(s) are raised)

n pending(s)
  n test case(s) are pending (self.pend() is used n times)

n notification(s)
  n notification(s) are occurred (self.notify() is used n times)

In the example, 35 test cases are run, 55 assertions are
passed and an assertion is failed. There are no error,
pending, notification.

XML report
----------

Pikzie reports test result as XML format if --xml-report
option is specified. A reported XML has the following
structure::

  <report>
    <result>
      <test-case>
        <name>TEST CASE NAME</name>
        <description>DESCRIPTION OF TEST CASE (if exists)</description>
      </test-case>
      <test>
        <name>TEST NAME</name>
        <description>DESCRIPTION OF TEST CASE (if exists)</description>
        <option><!-- ATTRIBUTE INFORMATION (if exists) -->
          <name>ATTRIBUTE NAME (e.g.: bug)</name>
          <value>ATTRIBUTE VALUE (e.g.: 1234)</value>
        </option>
        <option>
          ...
        </option>
      </test>
      <status>TEST RESULT ([success|failure|error|pending|notification])</status>
      <detail>DETAIL OF TEST RESULT (if exists)</detail>
      <backtrace><!-- BACKTRACE (if exists) -->
        <entry>
          <file>FILE NAME</file>
          <line>LINE</line>
          <info>ADDITIONAL INFORMATION</info>
        </entry>
        <entry>
          ...
        </entry>
      </backtrace>
      <elapsed>ELAPSED TIME (e.g.: 0.000010)</elapsed>
    </result>
    <result>
      ...
    </result>
    ...
  </report>

References
==========

Options
-------

See "Template" section in this document how to pass options to Pikzie.

--version               shows its own version and exits.

-pPATTERN, --test-file-name-pattern=PATTERN collects test
                                            files that
                                            matches with the
                                            specified glob
                                            pattern.

                                            Default: test/test_*.py

-nTEST_NAME, --name=TEST_NAME  runs tests that are matched
                               with TEST_NAME. If TEST_NAME
                               is surrounded by "/"
                               (e.g. /test\_/), TEST_NAME is
                               handled as regular
                               expression.

                               This option can be specified
                               n times. In the case, Pikzie
                               runs tests that are matched
                               with any TEST_NAME.

-tTEST_CASE_NAME, --test-case=TEST_CASE_NAME  runs test
                                              cases that are
                                              matched with
                                              TEST_CASE_NAME.
                                              If
                                              TEST_CASE_NAME
                                              is surrounded
                                              by "/"
                                              (e.g. /TestMyLib/),
                                              TEST_CASE_NAME
                                              is handled as
                                              regular
                                              expression.

                                              This option
                                              can be
                                              specified n
                                              times. In the
                                              case, Pikzie
                                              runs test
                                              cases that are
                                              matched with
                                              any
                                              TEST_CASE_NAME.

--xml-report=FILE         outputs test result in XML format
                          to FILE.

--priority                selects tests to run according to
                          their priority. If a test is not
                          passed in the previous test, the
                          test is ran.

--no-priority             runs all tests regardless of their
                          priority. (default)

-vLEVEL, --verbose=LEVEL  specifies verbose level. LEVEL is
                          one of [s|silent|n|normal|v|verbose].

                          This option is only for console
                          UI. (There is only console UI at
                          present.)

-cMODE, --color=MODE      specifies whether colorize output
                          or not. MODE is one of
                          [yes|true|no|false|auto]. If 'yes'
                          or 'true' is specified, colorized
                          output by escape sequence is used.
                          If 'no' or 'false' is specified,
                          colorized output is never used. If 'auto'
                          or the option is omitted,
                          colorized output is used if available.

                          This option is only for console
                          UI. (There is only console UI at
                          present.)

--color-scheme=SCHEME     specifies whether color scheme is
                          used for output. SCHEME is one of
                          [default].

                          This option is only for console
                          UI. (There is only console UI at
                          present.)

Assertions
----------

Use pydoc::

  % pydoc pikzie.assertions.Assertions

Or you can see HTML version on the Web:
http://pikzie.sourceforge.net/assertions.html

Attribute
---------

You can add attributes to your test to get more useful
information on failure. For example, you can add Bug ID like
the following::

  import pikzie

  class TestYourModule(pikzie.TestCase):
      @pikzie.bug(123)
      def test_invalid_input(self):
          self.assert_call_raise(IndexError, ().__getitem__, 0)

In the above example, test_invalid_input test has an attribute
that the test is for Bug #123.

Here is a list of available attributes:

pikzie.bug(id)
  Set id as Bug ID.

pikzie.priority(priority)
  Decide to whether run the test or not according to the
  priority. Here are the available priorities. If
  --no-priority command line option is specified, the
  priority is not used.

  must
    must run the test.

  important
    run the test at the probability of 90 percent.

  high
    run the test at the probability of 70 percent.

  normal
    run the test at the probability of 50 percent. (default)

  low
    run the test at the probability of 25 percent.

  never
    never run the test.


Thanks
------

- atzm:
	reports a bug.

	makes a ebuild. http://diary.atzm.org/20081201.html#p01

- Hideo Hattori:
	reports a bug.
