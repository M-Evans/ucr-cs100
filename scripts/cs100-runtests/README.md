# How to use ``cs100-runtests``

## What is this?
This is a guide designed to help familiarize you with the ``cs100-runtests`` utility.
It is broken up into six sections: an overview, examples for each homework assignment, and an FAQ.

### Overview
This section details ``cs100-runtests`` syntax.
For examples and real use cases, see each assignment section.

``cs100-runtests`` accepts 2 or more parameters.
  1. The file[s] containing tests you'd like to run.
  * This parameter undergoes [filename expansion](https://www.gnu.org/software/bash/manual/html_node/Filename-Expansion.html).
  * If one of the expanded terms is a directory, all of the contents of that directory are used as tests.
  * ``cs100-runtests`` will print an error message for each empty directory, and continue with the rest of the terms.
  
  **Hint**: use echo to see what will be used as tests.
  e.g. ``echo "tests/pip*"`` will show you what files and directories are [expanded](https://www.gnu.org/software/bash/manual/html_node/Filename-Expansion.html) from ``pip``.

  2. The number of microseconds to wait between feeding commands to the chosen shell.

  * This parameter is optional, and defaults to ``300000`` (0.3 seconds).
  * It is useful to set this to a larger value if commands in a test file take longer than 0.3 seconds to finish.

  3. The shell you'd like to run tests in.

  * All the tests you specify with the first argument will be sent to and executed by this program.
  * Some examples include ``bash``, ``sh``, or even your very own ``rshell``

  **Hint**: A good rule of thumb to make sure you're specifying this parameter properly is as follows:
  If you can execute it, ``cs100-runtests`` can use it to run the tests.

  4. If arguments are supplied at this point, they are used as arguments to whatever shell was specified.

  **Hint**: In the command ``cs100-runtests tests/myTest bash -i -c "echo 'hello world'"``, ``cs100-runtests`` is able to detect that you didn't specify an amount of time to wait between commands so it uses the default.
  Also, ``bash -i -c "echo 'hello world'"`` will recieve the input from the file.


## hw0
In [hw0](https://github.com/mikeizbicki/ucr-cs100/#course-schedules), you are required to create a basic shell that accepts commands and executes them.
Additionally, you must implement ``&&``, ``||``, and ``;`` as connectors.
Here's how you would run your tests for hw0:

``cs100-runtests bin/rshell tests/exec``

[tests/execExampleTest1](tests/execExampleTest1) shows what a test file would look like and [tests/execExampleTest1_Output](tests/execExampleTest1_Output) shows what the corresponding output would be.

Another requirement for hw0 is that you implement the ``exit`` command.
[tests/execExampleTest2](tests/execExampleTest2) is one example of a test file for ``exit`` in ``rshell``.
There is a problem, however.
When ``rshell`` is finished executing, **none of the remaining tests will be passed**.
To test exit multiple times, you need to specify multiple test files.
[tests/execExampleTest3](tests/execExampleTest3) contains an example of another distinct test for ``exit`` in ``rshell``.

If, in your tests, you do not cause ``rshell`` to stop with ``exit`` then it must finish when there is no more input to be had.
If you're using ``cin``, ``cin.good()`` will return ``true`` if there isn't a problem with the ``cin`` stream.
When ``cs100-runtests`` is finished feeding lines of input to your ``rshell``, ``cin.good()`` will return ``false``.
Similarly, ``cin.fail()`` will return ``true`` when there's no more input.

### hw1
In [hw1](https://github.com/mikeizbicki/ucr-cs100/#course-schedules), you're required to implement the ``ls`` command.
Here's how you would run your tests for hw1: ``cs100-runtests bash tests/ls``.
[tests/lsExampleTest](tests/lsExampleTest) contains a set of example test cases for hw1.

The main difference between testing hw1 and hw0 is the shell you're running your tests through.
If you'd like to run your tests through ``rshell``, pass the path to your ``rshell`` executable as the second argument.
If you'd like to run your tests through ``bash``, pass ``bash`` instead.
For this assignment, you are not required to run your tests through ``rshell``.

### hw2
In [hw2](https://github.com/mikeizbicki/ucr-cs100/#course-schedules), you're required to implement several different piping and redirection features that use ``<``, ``>``, ``>>``, and ``|`` as part of their syntax.
Here's how you would run your tests for hw2: ``cs100-runtests bin/rshell tests/piping``.
[tests/pipingExampleTest](tests/pipingExampleTest) contains a set of example test cases for hw2.

### hw3
In [hw3](https://github.com/mikeizbicki/ucr-cs100/#course-schedules), you're required to execute commands by searching the ``PATH`` environment variable, catch and handle when the user types ``Ctrl+c``, and implement ``cd``.
The extra credit is given for handling when the user types ``Ctrl+z``, as well as implementing ``fg`` and ``bg``.
Here's how you would run your tests for hw3: ``cs100-runtests bin/rshell tests/signals``.
[tests/signalsExampleTest](tests/signalsExampleTest) contains a set of example test cases for hw3.

You must put the [ASCII end of text character](http://en.wikipedia.org/wiki/End-of-text_character) in the test file to send ``SIGINT`` to your ``rshell``.
[Ctrl+C, used for interrupting a running program, overlaps with this character.](http://en.wikipedia.org/wiki/Control-C)
To place an ASCII end of text character in vim, enter insert mode and hit ``Ctrl+v`` then ``Ctrl+c``.

For the extra credit, you will need to handle ``^Z``, which represents sending ``SIGTSTP`` to your shell.
It overlaps with the [ASCII substitute character.](http://en.wikipedia.org/wiki/Substitute_character)
To have ``cs100-runtests`` send ``SIGTSTP`` to your ``rshell``, you must put the ASCII substitute character in the test file.
To place an ASCII substitute character in vim, enter insert mode and hit ``Ctrl+v`` then ``Ctrl+z``.

These control characters must be on a line by themselves in order for ``cs100-runtests`` to send the corresponding signal.

Signals can be sent to ``bash``, but ``bash`` does not relay them to the currently running program.


