# 7. Ensuring Linting & Type Checking :coffee:

:house: [Overview](../../README.md)

<br>
<br>

## Linting with Flake8 

<br>


[**Flake8**](https://flake8.pycqa.org/en/latest/) is a library that highlights syntactical and stylistic errors in python source code. It significantly improves upon the readability of your source code by enforcing the [PEP8](https://peps.python.org/pep-0008/) standards. 

Example of possible error that could be identified are the below 

- unused import
- inconsistency in indentation
- line of code is too long ...

In order to install **flake8** run the below command in the terminal

```sh
$ pip install flake8
```

Then scan your source code by running the below

```sh
$ flake8 src
```

:information_source:  : Here we defined our directory **src** to be the folder of interest for linting. 


<br>
<br>

## Type Checking with Mypy  

<br>

[**Mypy**](https://mypy.readthedocs.io/en/stable/) is a great tool for static type-checking that help ensure that variables and functions types remain consistent throughout your source code when explicitly defined. Thus, it significantly improves the quality of your code by proactively detecting all typing-related bugs in Python according to the [PEP484](https://peps.python.org/pep-0484/).

Let's take the case below 

```python 
def addition(a:int, b:int) -> int:
    result:float = 0.0
    result = a + b
    return result 

name="Etienne"
age= 4.5 
addition(name, age)

```

Here, **mypy** will raise several type-related bugs as we explicitly defined the types of the arguments that the function **addition** accepts.  Also there will be a flag raised on the inconsistencies between the return type defined (**int**) and the type of the variable returned (**float**)

<br>

To install **mypy** run the below command in the terminal

```sh
$ pip install mypy
```


Then scan your source code by running the below in your terminal

```sh
$ mypy src
```

In some cases, external type checking libs are required to scan the code. You will have to run the type-checking analysis with the flags `install-types`  and `non-interactive`  
```sh
$ mypy --install-types --non-interactive src
```

:information_source:  : Here we defined our directory **src** to be the folder of interest for type-checking. 

<br>


:thought_balloon:: Make sure to include  **flake8** and **mypy** as part of the dependencies to the file  **dev-requirements.txt**  

<br>
<br>

---


[ << (  6. Writing Unit Testing ) ](../chapters/chapter_6.md#get-ready-for-testing) &nbsp;&nbsp; |  &nbsp;&nbsp;  [ ( 8.Packaging with Setup.py ) >>](../chapters/chapter_8.md)  