# Programming Language: Python Loops, functions, modules and libraries

**Python**: *Loops*, *functions*, *modules* and *libraries* are all important concepts in **Python** programming.

- **Loops** are used to repeat a block of code a certain number of times or until a certain condition is met. There are two types of loops in Python: for loops and while loops.

- **Functions** are reusable blocks of code that can be called from anywhere in a program. Functions can take in arguments and return values.

- **Modules** are files that contain Python code. Modules can be imported into other programs to use their functions and variables.

- **Libraries** are collections of modules that are designed to perform specific tasks. There are many popular Python libraries, such as the math library, the random library, and the datetime library.

- **File I/O** in Python refers to the reading and writing of data to and from files. Files are containers that store data on a computer's hard drive or other storage device. They can be used to store a variety of data, such as text, images, and audio files.

## Loops (for/while):

**Loops** are used to repeatedly run a block of code.

### for loop

Using the `for` loop, a piece of code is executed once for each element of a sequence (such as a list, string, or tuple). Here is an example of a for loop that prints each programming language in a list:

``` python
languages = ['Python', 'Go', 'JavaScript']

# for loop
for language in languages:
    print(language)
```

Output
```
Python
Go
JavaScript
```

### while loop

The `while loop` is used to execute a block of code repeatedly as long as a condition is **True**. Here's an example of a while loop that prints the numbers from 1 to 5:

``` python
i = 1
n = 5

# while loop from i = 1 to 5
while i <= n:
    print(i)
    i = i + 1
```

Output:
```
1
2
3
4
5
```

## Functions

**Functions** are reusable chunks of code with argument/parameters and return values.
Using the `def` keyword in **Python**, you can define a function. In your programme, functions can be used to encapsulate complex logic and can be called several times.
**Functions** can also be used to simplify code and make it easier to read. Here is an illustration of a function that adds two numbers:

``` python
# function has two arguments num1 and num2
def add_numbers(num1, num2):
    sum = num1 + num2
    print('The sum is: ',sum)
```

``` python
# calling the function with arguments to add 5 and 2
add_numbers(5, 2)

# Output: The sum is: 9
```

## Understanding Modules and Importing Libraries:

**A module** is a file in **Python** that contains definitions and statements. Modules let you arrange your code and reuse it across many apps.

**The Standard Library**, a sizable collection of Python modules, offers a wide range of capabilities, such as **file I/O**, **regular expressions**, and more.

Additional libraries can be installed using package managers like pip.

You must import a module or library using the import statement in order to use it in your programme.

Here is an illustration of how to load the math module and calculate a number's square root using the sqrt() function:

``` python
import math

print(math.sqrt(16)) # 4.0
```

## File I/O

In **Python**, there are a number of built-in functions that can be used to read and write files. The most common function for opening a file is the *open()* function.

The *open()* function takes two arguments: *the name of the file to open* and *the mode in which the file should be opened*.

The mode can be one of the following:

- *r* - Read mode. This mode is used to read data from a file.
- *w* - Write mode. This mode is used to write data to a file.
- *a* - Append mode. This mode is used to append data to a file.

Once a file has been opened, it can be read or written to using a number of other functions. For example, the *read()* function can be used to read data from a file, and the *write()* function can be used to write data to a file.

Here is an example of how to read a file in **Python**:

``` Python
def read_file(filename):
    with open(filename, "r") as f:
        data = f.read()
    return data

data = read_file("myfile.txt")
print(data)
```

This code will open the file *myfile.txt* in read mode and then read the contents of the file into a string. The string containing the contents of the file is then printed to the console.

Here is an example of how to write a file in **Python**:

```Python
def write_file(filename, data):
    with open(filename, "w") as f:
        f.write(data)

data = "This is some data."
write_file("myfile.txt", data)
```

This code will open the file *myfile.txt* in write mode and then write the string This is some data. to the file.

## Exception Handling:

**Exceptions** are runtime errors that happen when your programme runs into unexpected circumstances, such dividing by zero or attempting to access a list element that doesn't exist.

Using a try/except block, you can manage exceptions in **Python**. The try block's code is run, and if an exception arises, the except block's code is run to handle the exception.

``` python
try:
  f = open("myfile.txt")
  try:
    f.write("Python is great")
  except:
    print("Something went wrong when writing to the file")
```

## Conclusion

## Resources:

- [7 Days of Python](https://7daysofpython.com/)