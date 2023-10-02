Documentation by Phillip Zenger @ Computational Thinking Lab ETH Zurich

# Errors in Python

1. [Introduction](#introduction)
2. [SyntaxError](#syntaxerror)
3. [IndentationError](#indentationerror)
4. [TypeError](#typeerror)
5. [IndexError](#indexerror)
6. [NameError](#nameerror)
7. [ValueError](#valueerror)
8. [AttributeError](#attributeerror)
9. [FileNotFoundError](#filenotfounderror)

## Introduction

One will inevitably encounter errors when programming – this is only natural. When coding in python, these errors will fall into three categories: syntax, runtime and logical errors. Of these three, the third category, logical errors, is generally the hardest to resolve. It is also difficult to summaries because these problems heavily depend on the code. Consequently, this document will focus on syntax and runtime errors in an attempt of providing an overview, including steps on how to deal with the errors.

Both syntax and runtime errors will break the code, leading to an error message in the console (Windows: command line; Mac/Linux: terminal) where the code execution happens. Below there are two general examples, to given an introduction to reading these kind of messages. (Logical errors on the other hand will often not break the code, but cause unexpected results.)

### Example 1

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
Traceback (most recent call last):
  File "/Users/student/Developer/raising_errors.py", line 7, in <module>
    all_lines = read_txt_file(name)
                ^^^^^^^^^^^^^^^^^^^
  File "/Users/student/Developer/raising_errors.py", line 2, in read_txt_file
    with open(f'{filename}.txt') as f:
         ^^^^^^^^^^^^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: 'invalidfilename.txt'
```

In this first example, the 'open()' function raises the runtime error 'FileNotFoundError'. This error occurs when trying to access a file with a name that doesn't exist, but also when looking for the file in the wrong location. Here python failed to locate 'invalidfilename.txt'.

The first line is the command which runs the .py file. The '(base)' environment is activated under the 'student' account and python from 'miniconda3/bin/python' is used to run 'raising_errors.py' in the folder 'Developer'. The current working directory is 'Developer' which is stated after the account name. (This command can look very different depending on the python installation, the users file structure and the operating system.)

The Traceback underneath shows the order in which the code was executed before the error was encountered. Circumflex symbols ('^') underline the exact function that was called in each line. In this example, the function 'read_txt_file()' is called with the argument 'name' in line 7 of the file. Inside of this function, python tries to access the file called '{filename}.txt', where 'filename' is a local variable, using python's own 'open()' function. All the code behaves as expected, but python cannot find a file or directory, which means folder structure, with the name provided to the 'open()' function.

A possible strategy on how to correct and even avoid this error in the first place can be found in the FileNotFoundError section below.

<!-- Text -->
### Example 2

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/ttp.py
Traceback (most recent call last):
  File "/Users/student/Developer/ttp.py", line 23, in <module>
    ax.set(x_label='time [t]', y_label='distance [arbitrary units]')
  File "/Users/student/miniconda3/lib/python3.11/site-packages/matplotlib/artist.py", line 147, in <lambda>
    cls.set = lambda self, **kwargs: Artist.set(self, **kwargs)
                                     ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/student/miniconda3/lib/python3.11/site-packages/matplotlib/artist.py", line 1231, in set
    return self._internal_update(cbook.normalize_kwargs(kwargs, self))
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/student/miniconda3/lib/python3.11/site-packages/matplotlib/artist.py", line 1223, in _internal_update
    return self._update_props(
           ^^^^^^^^^^^^^^^^^^^
  File "/Users/student/miniconda3/lib/python3.11/site-packages/matplotlib/artist.py", line 1197, in _update_props
    raise AttributeError(
AttributeError: Axes.set() got an unexpected keyword argument 'x_label'
```

In this second example, a numpy function called 'axes.set()' with the intention of adding labels to a plot, but the keyword argument is wrong. This information can be found in the last line where it says 'unexpected keyword argument', which is followed by the keyword the code failed to interpret. Passing a variable to a function under the wrong name, prevents the funcion from doing anything with said variable and will raise the runtime error 'AttributeError'.

Most of the Traceback relates to code in the 'artist.py' file located in the *matplotlib* library. This file handles most of the plot generation, but the actual cause of the error can be found at the top of the Traceback. Here one can find the line number in addition to the code where the erroneous function call originates. All the other lines of code called are part of the *matplotlib* library. (While it is not entirely impossible to encounter an error in a library, most undergo thorough testing before being released and therefore one can generally assume that the error was caused by improper use.)

To fix this particular error, the correct variable name must be used. The name for the x axis label is called 'xlabel' which can be found in the documentation of numpy. Additionally, the second variable name is also spelled incorrectly (correct: 'ylabel'), but code execution was stopped immediately after the first error was encountered.

### Python Linting in VS Code

VS Code will try to assist the user by highlighting errors ahead of time and sometimes even providing limited instructions on how to resolve the issue. This is called linting and can be very useful when dealing with syntax errors. However, it has rather limited capabilities when dealing with errors that relate to function calls, especially in imported code and libraries, because it doesn't know which values to expect as arguments. (Some developers like to use type hints to be more explicit with their use of variables, but this is outside the scope of this document.)

## SyntaxError

Anything lines, words and single characters that python doesn't recognise as code when trying to run the program will raise a SyntaxError. As implied by the name, this error falls in the syntax category.

```py
greeting equals 'hello'
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
  File "/Users/student/Developer/raising_errors.py", line 1
    greeting equals hello
             ^^^^^^
SyntaxError: invalid syntax
```

```py
greeting = 'hello'
```

Oftentimes one can find a suggestion on how to fix the error. In the example below the assignment opterator ('=') was used instead of the comparison operator ('==').

```py
x = 2

if x = 5:
    print('five')
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
  File "/Users/student/Developer/raising_errors.py", line 5
    if x = 5:
       ^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```

```py
x = 2

if x == 5:
    print('five')
```

## IndentationError

Python doesn't rely on curly brackets or similar syntax to separate code blocks (as well as the scope), and instead uses indentations for this purpose. This error also falls in the syntax category.

While any number of spaces will do, it is best practice to use 4 spaces, which will also improve readability. Not having any spaces or inconsitent use of spaces will raise an IndentationError.

```py
for i in range(5):
print('errors: ' + str(i))
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
  File "/Users/student/Developer/raising_errors.py", line 2
    print('errors: ' + str(i))
    ^
IndentationError: expected an indented block after 'for' statement on line 1
```

```py
for i in range(5):
  print('errors: ' + str(i))
```

This will also happen if there is no code in the next line at all. Something that can happen easily when multiple people are working on the same file at the same time. To mitigate this issue, the 'pass' statement can be used. The 'pass' statement is ignored when the code is run, but still fullfills the requirement of having at least one statement inside an identation block. In the case of functions an empty return statement will also do the trick.

```py
# function discription
def new_function():

print('Welcome')
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
  File "/Users/student/Developer/raising_errors.py", line 4
    print('Welcome')
    ^
IndentationError: expected an indented block after function definition on line 2
```

```py
# function discription
def new_function():
  pass

print('Welcome')
```

Note: Switching between tabs and spaces as indentations in one python file will raise a separate but related error called 'TabError'. This is most often encountered when copying  code that is saved in a different file format into a code editor. By default, VS Code will use 4 spaces per indentation.

## TypeError

Many operations in python are designed to work with specific data types. Whenever a data type is incompatible with an operation python will raise the runtime error TypeError. Luckily, this error is highlighted by VS Code in advance.

```py
print('errors = ' + 1)
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
Traceback (most recent call last):
  File "/Users/student/Developer/raising_errors.py", line 1, in <module>
    print('errors = ' + 1)
          ~~~~~~~~~~~~^~~
TypeError: can only concatenate str (not "int") to str
```

```py
print('errors = ' + str(1))
```

It is also important to remember, that all inputs are considered strings. If the expected input value is a number, make sure to convert it to the appropriate data type and ideally account for incorrect input values by handling exceptions explicitly.

<!-- Consider discussing consequences of mixing data types here. -->

## IndexError

<!-- Attemting to access an element that does not exist. Reiterate how list indices works in python. -->
Working with tuples, lists, etc. in python can be very useful, but sometimes it can be a bit tricky to find the correct indices. Attempting to access an element that does not exist will raise the runtime error 'IndexError'. While on the subject, it is important to remember that the first element always has index *0* and therefore the index of the last element corresponds to the *total number of elements* - 1. Also, using a negative number as index will start from the back.

```py
data = [1, 5, 8, 10]

print('Last element: ' + str(data[4]))
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
Traceback (most recent call last):
  File "/Users/student/Developer/raising_errors.py", line 3, in <module>
    print('Last element: ' + str(data[4]))
                                 ~~~~^^^
IndexError: list index out of range
```

```py
data = [1, 5, 8, 10]

print('Last element: ' + str(data[-1]))
```

## NameError

<!-- Discuss scopes. These errors can be a little tricky to identify, because they can have different origins. -->
Python is not a 'statically typed' programming language, meaning variables can be defined and used freely without being bound to a type. When using python, a variable is created the instant a value is asigned to it using an assignment operator.

Another important thing to note about variables is their scope. The scope of a variable determines where it can be accessed and can impact its current value. Within python, the LEGB-rule governs the scope of variables. This abbreviation stands for local, enclosed, global & built-in, which corresponds to the order in which python will check if a variable exists.

- Both local and enclosing scopes relate to variables set inside a function with affecting nested functions similar to global variables.
- Global variables are not defined in any function and can be called throughout the code, but doing so inside a function can make code hard to read and difficult troubleshoot. As a general rule, all objects required inside a function should be passed to it as arguments.
- Built-in variables are reserved by modules or libraries. Their value can be overwritten by having a variable with the same name in one of the other scopes.

For further reading, please refer to [this GeeksForGeeks](https://www.geeksforgeeks.org/scope-resolution-in-python-legb-rule/) article.

```py
# function discription
def new_function():
    new_variable = 1

print(new_variable)
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
Traceback (most recent call last):
  File "/Users/student/Developer/raising_errors.py", line 5, in <module>
    print(new_variable)
          ^^^^^^^^^^^^
NameError: name 'new_variable' is not defined
```

```py
# function discription
def new_function():
    new_variable = 1

print(new_variable)
```

This error can also occure when trying to access a variable that is not defined at all. Coincidentally, forgetting to enclose a sting in single or double quation marks can trigger this problem because python will treat any group of characters starting with a letter a variable name. If a variable with the given name exists, it's value could be accidentially used in the wrong place or even change.

```py
print(error)
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
Traceback (most recent call last):
  File "/Users/student/Developer/raising_errors.py", line 1, in <module>
    print(error)
          ^^^^^
NameError: name 'error' is not defined. Did you mean: 'OSError'?
```

```py
print('error')
```

## ValueError

<!-- Passing a value to a function that is not allowed. Also applies to incorrect type conversions. By default all inputs are considered strings. -->

When inserting arguments into a function call, it is often important to choose the correct data type and format. For example, the function 'sqrt()' from the math library will raise a 'ValueError' at runtime when recieving a negative number.

```py
import math

print(math.sqrt(-1))
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
Traceback (most recent call last):
  File "/Users/student/Developer/raising_errors.py", line 3, in <module>
    print(math.sqrt(-1))
          ^^^^^^^^^^^^^
ValueError: math domain error
```

```py
import math

print(math.sqrt(abs(-1)))
```

<!-- Other cases exist... -->

## AttributeError

<!-- Calling an attribute that doesn't exist for a given object. -->

If a function is called on a variable of the wrong data type, python will raise the 'ValueError' at runtime. Sometimes a variable can change data type and it can therefore be useful to explicitly check what data type is given. In other cases, where only a single type is expected, the data type must be changed or an alternative function found.

```py
not_a_list = '1, 5, 1, 7, 9'
not_a_list.sort()
```

```console
(base) student Developer % /Users/student/miniconda3/bin/python /Users/student/Developer/raising_errors.py
Traceback (most recent call last):
  File "/Users/student/Developer/raising_errors.py", line 2, in <module>
    not_a_list.sort()
    ^^^^^^^^^^^^^^^
AttributeError: 'str' object has no attribute 'sort'
```

```py
not_a_list = '1, 5, 1, 7, 9'
now_a_list = not_a_list.split(', ')
now_a_list.sort()
```

Another example, where a keyword argument was misspelled in a function call, has been discussed in the introduction section.

## FileNotFoundError

*An example for this kind of error can be found at the top of this file in the introduction section.*

While the problem appears relatively straight forward, it can sometimes be a bit tricky to indentify exactly why the file in question cannot be found if the correct name is provided. Often this relates to the path being incomplete, which causes the function to look in the wrong place. Unfortunately the output above gives very little indication of which folder python was searching in. Generally python will look in one of following locations: the folder where the .py file is located, the (top most) folder of the workspace called current working directory, the folder of the current user account or the root folder of the operating system.

A slightly more advanced strategy for handling paths in python will be discussed below. However the simplest approach when working only on a single machine is to always insert the absolute path. This can be done by selecting 'Copy Path' (de: 'Pfad kopieren') after right clicking on the desired file. However, the use of static paths is discourraged.

The absolute path always depends on the local file structure and no two users will have exactly the same configuration. It is therefore considered best pratice to use python's built-in *os* module which handles 'micelaneous operating system interfaces' or alternatively the *pathlib* model which does 'object-oriented filesystem paths'. Examples for working with paths using os and pathlib may be provided in a separate document. See the [CTL main page](ctl.polyphys.mat.ethz.ch) for updates.

<!-- Optionally discuss: RecursionError,  -->
