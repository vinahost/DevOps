# Python: Debugging, Testing and Regular expression

**Debugging**, **testing**, and **regular expressions** are all important aspects of programming in **Python**.

- **Debugging** is the process of ensuring that code is working properly. It is also an essential part of software development, as it helps to ensure that the code is meeting the requirements of the project. There are a number of different testing techniques that can be used, such as unit testing, integration testing, and system testing.

- **Testing** is the process of ensuring that code is working properly. It is also an essential part of software development, as it helps to ensure that the code is meeting the requirements of the project. There are a number of different testing techniques that can be used, such as unit testing, integration testing, and system testing.

- **Regular expressions** is a built-in Python library that provides a number of functions for working with dates and times. The datetime library can be used to represent dates, times, and time intervals. It can also be used to perform calculations with dates and times, such as finding the difference between two dates or converting between different date and time formats.

- **Datetime library** is a built-in **Python** library that provides a number of functions for working with dates and times. The datetime library can be used to represent dates, times, and time intervals. It can also be used to perform calculations with dates and times, such as finding the difference between two dates or converting between different date and time formats.

## Debugging and testing

**Debugging** is the process of finding and correcting errors or bugs in code. **Python** includes a debugger called `pdb` that allows you to step through your code and inspect variables as you go. You can use `pdb` to help you figure out where your code is going wrong and how to fix it.

There are also a number of online tools that can help you debug your code, such as **Debuggex**: https://www.debuggex.com/ and Pythex: https://pythex.org/.

``` python
import pdb

def add_numbers(x, y):
    result = x + y
    pdb.set_trace() # Start the debugger at this point in the code
    return result

result = add_numbers(2, 3)
print(result)
```

In this example, we define the `add_numbers` function, which adds two numbers and returns the result. To start the debugger at a specific point in the code, we use the pdb.set trace() function (in this case, after the result has been calculated). This enables us to inspect variables and step through the code to figure out what's going on.

In addition to debugging, testing is an important part of programming. It entails creating test cases to ensure that your code is working properly. **Python** includes a `unittest` module that provides a framework for writing and running test cases.


``` python
import unittest

def is_prime(n):
    if n < 2:
        return False
    for i in range(2, n):
        if n % i == 0:
            return False
    return True

class TestIsPrime(unittest.TestCase):
    def test_is_prime(self):
        self.assertTrue(is_prime(2))
        self.assertTrue(is_prime(3))
        self.assertTrue(is_prime(5))
        self.assertFalse(is_prime(4))

if __name__ == '__main__':
    unittest.main()

```

Output:

``` bash
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
```

## Regular expressions:

In **Python**, regular expressions are a powerful tool for working with text data. They enable you to search for and match specific character patterns within a string. Python's `re` module includes functions for working with regular expressions.

For example, you can use regular expressions to search for email addresses within a larger block of text, or to extract specific data from a string that follows a particular pattern.

``` python
import re

# Search for a phone number in a string
text = 'My phone number is 555-7777'
match = re.search(r'\d{3}-\d{4}', text)
if match:
    print(match.group(0))

# Extract email addresses from a string
text = 'My email is example@devops.com, but I also use other@cloud.com'
matches = re.findall(r'\S+@\S+', text)
print(matches)
```

Output:

``` bash
555-7777
['example@devops.com,', 'other@cloud.com']
```

## Datetime library:

As the name suggests, Python's `datetime` module allows you to work with dates and times in your code. It includes functions for formatting and manipulating date and time data, as well as classes for representing dates, times, and time intervals.

The datetime module, for example, can be used to get the current date and time, calculate the difference between two dates, or convert between different date and time formats.

``` python
from datetime import datetime, timedelta

# Get the current date and time
now = datetime.now()
print(now) # Output: 2023-02-17 11:33:27.257712

# Create a datetime object for a specific date and time
date = datetime(2023, 2, 1, 12, 0)
print(date) # Output: 2023-02-01 12:00:00

# Calculate the difference between two dates
delta = now - date
print(delta) # Output: 15 days, 23:33:27.257712
```

Output:

``` bash
2023-02-17 11:33:27.257712
2023-02-01 12:00:00
15 days, 23:33:27.257712
```

### Sumary

Here are some examples of how to use these tools in **Python**:

- To debug a **Python** program, you can use the *pdb* debugger. The *pdb* debugger allows you to step through your code line by line, inspect variables, and set breakpoints.
- To test a **Python** program, you can use the **unittest** unit testing framework. The **unittest** framework provides a number of functions for writing and running unit tests.
- To use regular expressions in **Python**, you can use the *re* module. The *re* module provides a number of functions for searching for and matching character patterns in strings.
- To use the datetime library in **Python**, you can import the *datetime* module. The *datetime* module provides a number of classes for representing dates, times, and time intervals.

## Resources

- **Debugging in Python**: https://docs.python.org/3/library/pdb.html
- **Testing in Python**: https://docs.python.org/3/library/unittest.html
- **Regular Expressions in Python**: https://docs.python.org/3/library/re.html
- **datetime - Basic date and time types**: https://docs.python.org/3/library/datetime.html
- [7 Days of Python](https://7daysofpython.com/days/day4/)