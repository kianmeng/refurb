<!--
Autogenerated! Do not modify!

See the "Updating Documentation" section of the README file for more info.
-->

# Available Checks

## FURB100: `use-pathlib-with-suffix`

Categories: `pathlib`

A common operation is changing the extension of a file. If you have an
existing `Path` object, you don't need to convert it to a string, slice
it, and append a new extension. Instead, use the `with_suffix()` method:

Bad:

```python
new_filepath = str(Path("file.txt"))[:4] + ".md"
```

Good:

```python
new_filepath = Path("file.txt").with_suffix(".md")
```

## FURB101: `use-pathlib-read-text-read-bytes`

Categories: `pathlib`

When you just want to save the contents of a file to a variable, using a
`with` block is a bit overkill. A simpler alternative is to use pathlib's
`read_text()` function:

Bad:

```python
with open(filename) as f:
    contents = f.read()
```

Good:

```python
contents = Path(filename).read_text()
```

## FURB102: `use-startswith-endswith-tuple`

Categories: `string`

`startswith()` and `endswith()` both take a tuple, so instead of calling
`startswith()` multiple times on the same string, you can check them all
at once:

Bad:

```python
name = "bob"
if name.startswith("b") or name.startswith("B"):
    pass
```

Good:

```python
name = "bob"
if name.startswith(("b", "B")):
    pass
```

## FURB103: `use-pathlib-write-text-write-bytes`

Categories: `pathlib`

When you just want to save some contents to a file, using a `with` block is
a bit overkill. Instead you can use pathlib's `write_text()` method:

Bad:

```python
with open(filename, "w") as f:
    f.write("hello world")
```

Good:

```python
Path(filename).write_text("hello world")
```

## FURB104: `use-pathlib-cwd`

Categories: `pathlib`

A modern alternative to `os.getcwd()` is the `Path.cwd()` method:

Bad:

```python
cwd = os.getcwd()
```

Good:

```python
cwd = Path.cwd()
```

## FURB105: `simplify-print`

Categories: `builtin` `readability`

`print("")` can be simplified to just `print()`.

## FURB106: `use-expandtabs`

Categories: `string`

If you want to expand the tabs at the start of a string, don't use
`.replace("\t", " " * 8)`, use `.expandtabs()` instead. Note that this
only works if the tabs are at the start of the string, since `expandtabs()`
will expand each tab to the nearest tab column.

Bad:

```python
spaces_8 = "\thello world".replace("\t", " " * 8)
spaces_4 = "\thello world".replace("\t", "    ")
```

Good:

```python
spaces_8 = "\thello world".expandtabs()
spaces_4 = "\thello world".expandtabs(4)
```

## FURB107: `use-with-suppress`

Categories: `contextlib` `readability`

Often times you want to handle an exception, and just ignore it. You can do
this with a `try/except` block, using a single `pass` in the `except`
block, but there is a simpler and more concise way using the `suppress()`
function from `contextlib`:

Bad:

```python
try:
    f()

except FileNotFoundError:
    pass
```

Good:

```python
with suppress(FileNotFoundError):
    f()
```

## FURB108: `use-in-oper`

Categories: `logical` `readability`

When comparing a value to multiple possible options, don't `or` multiple
comparison checks, use a single `in` expr:

Bad:

```python
if x == "abc" or x == "def":
    pass
```

Good:

```python
if x in ("abc", "def"):
    pass
```

Note: This should not be used if the operands depend on boolean short
circuiting, since the operands will be eagerly evaluated. This is primarily
useful for comparing against a range of constant values.

## FURB109: `use-consistent-in-bracket`

Categories: `iterable` `readability`

Since tuple, list, and set literals can be used with the `in` operator, it
is best to pick one and stick with it.

Bad:

```python
for x in (1, 2, 3):
    pass

nums = [str(x) for x in [1, 2, 3]]
```

Good:

```python
for x in (1, 2, 3):
    pass

nums = [str(x) for x in (1, 2, 3)]
```

## FURB110: `use-or-oper`

Categories: `logical` `readability`

Sometimes the ternary operator (aka, inline if statements) can be
simplified to a single `or` expression.

Bad:

```python
z = x if x else y
```

Good:

```python
z = x or y
```

Note: if `x` depends on side-effects, then this check should be ignored.

## FURB111: `use-func-name`

Categories: `readability`

Don't use a lambda if it is just forwarding its arguments to a
function verbatim:

Bad:

```python
predicate = lambda x: bool(x)

some_func(lambda x, y: print(x, y))
```

Good:

```python
predicate = bool

some_func(print)
```

## FURB112: `use-literal`

Categories: `pythonic` `readability`

Using `list` and `dict` without any arguments is slower, and not Pythonic.
Use `[]` and `{}` instead:

Bad:

```python
nums = list()
books = dict()
```

Good:

```python
nums = []
books = {}
```

## FURB113: `use-list-extend`

Categories: `list`

When appending multiple values to a list, you can use the `.extend()`
method to add an iterable to the end of an existing list. This way, you
don't have to call `.append()` on every element:

Bad:

```python
nums = [1, 2, 3]

nums.append(4)
nums.append(5)
nums.append(6)
```

Good:

```python
nums = [1, 2, 3]

nums.extend((4, 5, 6))
```

## FURB114: `no-double-not`

Categories: `builtin` `readability` `truthy`

Double negatives are confusing, so use `bool(x)` instead of `not not x`.

Bad:

```python
if not not value:
    pass
```

Good:

```python
if value:
    pass
```

## FURB115: `no-len-compare`

Categories: `iterable` `truthy`

Don't check a container's length to determine if it is empty or not, use
a truthiness check instead:

Bad:

```python
name = "bob"
if len(name) == 0:
    pass

nums = [1, 2, 3]
if len(nums) >= 1:
    pass
```

Good:

```python
name = "bob"
if not name:
    pass

nums = [1, 2, 3]
if nums:
    pass
```

## FURB116: `use-fstring-number-format`

Categories: `builtin` `fstring`

The `bin()`, `oct()`, and `hex()` functions return the string
representation of a number but with a prefix attached. If you don't want
the prefix, you might be tempted to just slice it off, but using an
f-string will give you more flexibility and let you work with negative
numbers:

Bad:

```python
print(bin(1337)[2:])
```

Good:

```python
print(f"{1337:b}")
```

## FURB117: `use-pathlib-open`

Categories: `pathlib`

When you want to open a Path object, don't pass it to `open()`, just call
`.open()` on the Path object itself:

Bad:

```python
path = Path("filename")

with open(path) as f:
    pass
```

Good:

```python
path = Path("filename")

with path.open() as f:
    pass
```

## FURB118: `use-operator`

Categories: `operator`

Don't write lambdas/functions to wrap builtin operators, use the `operator`
module instead:

Bad:

```python
from functools import reduce

nums = [1, 2, 3]

print(reduce(lambda x, y: x + y, nums))  # 6
```

Good:

```python
from functools import reduce
from operator import add

nums = [1, 2, 3]

print(reduce(add, nums))  # 6
```

## FURB119: `use-fstring-format`

Categories: `builtin` `fstring`

Certain expressions which are passed to f-strings are redundant because
the f-string itself is capable of formatting it. For example:

Bad:

```python
print(f"{bin(1337)}")

print(f"{ascii(input())}")

print(f"{str(123)}")
```

Good:

```python
print(f"{1337:#b}")

print(f"{input()!a}")

print(f"{123}")
```

## FURB120: `use-implicit-default`

Categories: 

Don't pass an argument if it is the same as the default value:

Bad:

```python
def greet(name: str = "bob") -> None:
    print(f"Hello {name}")

greet("bob")

{}.get("some key", None)
```

Good:

```python
def greet(name: str = "bob") -> None:
    print(f"Hello {name}")

greet()

{}.get("some key")
```

## FURB121: `use-isinstance-issubclass-tuple`

Categories: `python310` `readability`

`isinstance()` and `issubclass()` both take tuple arguments, so instead of
calling them multiple times for the same object, you can check all of them
at once:

Bad:

```python
if isinstance(num, float) or isinstance(num, int):
    pass
```

Good:

```python
if isinstance(num, (float, int)):
    pass
```

Note: In Python 3.10+, you can also pass type unions as the second param to
these functions:

```python
if isinstance(num, float | int):
    pass
```

## FURB122: `use-writelines`

Categories: `builtin` `readability`

When you want to write a list of lines to a file, don't call `.write()`
for every line, use `.writelines()` instead:

Bad:

```python
lines = ["line 1\n", "line 2\n", "line 3\n"]

with open("file") as f:
    for line in lines:
        f.write(line)
```

Good:

```python
lines = ["line 1\n", "line 2\n", "line 3\n"]

with open("file") as f:
    f.writelines(lines)
```

Note: If you have a more complex expression then just `lines`, you may
need to use a list comprehension instead. For example:

```python
f.writelines(f"{line}\n" for line in lines)
```

## FURB123: `no-redundant-cast`

Categories: `readability`

Don't cast a variable or literal if it is already of that type. This
usually is the result of not realizing a type is already the type you want,
or artifacts of some debugging code. One example of where this might be
intentional is when using container types like `dict` or `list`, which
will create a shadow copy. If that is the case, it might be preferable
to use `.copy()` instead, since it makes it more explicit that a copy
is taking place.

Examples:

Bad:

```python
name = str("bob")
num = int(123)

ages = {"bob": 123}
copy = dict(ages)
```

Good:

```python
name = "bob"
num = 123

ages = {"bob": 123}
copy = ages.copy()
```

## FURB124: `use-comparison-chain`

Categories: `logical` `readability`

When checking that multiple objects are equal to each other, don't use
an `and` expression. Use a comparison chain instead, for example:

Bad:

```python
if x == y and x == z:
    pass

# and

if x is None and y is None
    pass
```

Good:

```python
if x == y == z:
    pass

# and

if x is y is None:
    pass
```

Note: if `x` depends on side-effects, then this check should be ignored.

## FURB125: `no-redundant-return`

Categories: `control-flow` `readability`

Don't explicitly return if you are already at the end of the control flow
for the current function:

Bad:

```python
def func():
    print("hello world!")

    return

def func2(x):
    if x == 1:
        print("x is 1")

    else:
        print("x is not 1")

        return
```

Good:

```python
def func():
    print("hello world!")

def func2(x):
    if x == 1:
        print("x is 1")

    else:
        print("x is not 1")
```

## FURB126: `simplify-return`

Categories: `control-flow` `readability`

Sometimes a return statement can be written more succinctly:

Bad:

```python
def index_or_default(nums: list[Any], index: int, default: Any):
    if index >= len(nums):
        return default

    else:
        return nums[index]

def is_on_axis(position: tuple[int, int]) -> bool:
    match position:
        case (0, _) | (_, 0):
            return True

        case _:
            return False
```

Good:

```python
def index_or_default(nums: list[Any], index: int, default: Any):
    if index >= len(nums):
        return default

    return nums[index]

def is_on_axis(position: tuple[int, int]) -> bool:
    match position:
        case (0, _) | (_, 0):
            return True

    return False
```

## FURB127: `no-with-assign`

Categories: `readability` `scoping`

Due to Python's scoping rules, you can use a variable that has gone "out
of scope" so long as all previous code paths can bind to it. Long story
short, you don't need to declare a variable before you assign it in a
`with` statement:

Bad:

```python
x = ""

with open("file.txt") as f:
    x = f.read()
```

Good:

```python
with open("file.txt") as f:
    x = f.read()
```

## FURB128: `use-tuple-unpack-swap`

Categories: `readability`

You don't need to use a temporary variable to swap 2 variables, you can use
tuple unpacking instead:

Bad:

```python
temp = x
x = y
y = temp
```

Good:

```python
x, y = y, x
```

## FURB129: `simplify-readlines`

Categories: `builtin` `readability`

When iterating over a file object line-by-line you don't need to add
`.readlines()`, simply iterate over the object itself. This assumes you
aren't passing an argument to readlines().

Bad:

```python
with open("file.txt") as f:
    for line in f.readlines():
        ...
```

Good:

```python
with open("file.txt") as f:
    for line in f:
        ...
```

## FURB130: `no-in-dict-keys`

Categories: `dict` `readability`

If you only want to check if a key exists in a dictionary, you don't need
to call `.keys()` first, just use `in` on the dictionary itself:

Bad:

```python
d = {"key": "value"}

if "key" in d.keys():
    ...
```

Good:

```python
d = {"key": "value"}

if "key" in d:
    ...
```

## FURB131: `no-del`

Categories: `builtin` `readability`

The `del` statement is commonly used for popping single elements from dicts
and lists, though a slice can be used to remove a range of elements
instead. When removing all elements via a slice, use the faster and more
succinct `.clear()` method instead.

Bad:

```python
names = {"key": "value"}
nums = [1, 2, 3]

del names[:]
del nums[:]
```

Good:

```python
names = {"key": "value"}
nums = [1, 2, 3]

names.clear()
nums.clear()
```

## FURB132: `use-set-discard`

Categories: `readability` `set`

If you want to remove a value from a set regardless of whether it exists or
not, use the `discard()` method instead of `remove()`:

Bad:

```python
nums = {123, 456}

if 123 in nums:
    nums.remove(123)
```

Good:

```python
nums = {123, 456}

nums.discard(123)
```

## FURB133: `no-redundant-continue`

Categories: `control-flow` `readability`

Don't explicitly continue if you are already at the end of the control flow
for the current for/while loop:

Bad:

```python
def func():
    for _ in range(10):
        print("hello world!")

        continue

def func2(x):
    for x in range(10):
        if x == 1:
            print("x is 1")

        else:
            print("x is not 1")

            continue
```

Good:

```python
def func():
    for _ in range(10):
        print("hello world!")

def func2(x):
    for x in range(10):
        if x == 1:
            print("x is 1")

        else:
            print("x is not 1")
```

## FURB134: `use-cache`

Categories: `functools` `python39` `readability`

Python 3.9 introduces the `@cache` decorator which can be used as a
short-hand for `@lru_cache(maxsize=None)`.

Bad:

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def f(x: int) -> int:
    return x + 1
```

Good:

```python
from functools import cache

@cache
def f(x: int) -> int:
    return x + 1
```

## FURB135: `no-ignored-dict-items`

Categories: `dict`

Don't use `.items()` on a `dict` if you only care about the keys or the
values, but not both:

Bad:

```python
books = {"Frank Herbert": "Dune"}

for author, _ in books.items():
    print(author)

for _, book in books.items():
    print(book)
```

Good:

```python
books = {"Frank Herbert": "Dune"}

for author in books:
    print(author)

for book in books.values():
    print(book)
```

## FURB136: `use-min-max`

Categories: `builtin` `logical` `readability`

Certain ternary expressions can be written more succinctly using the
builtin `min`/`max` functions:

Bad:

```python
score1 = 90
score2 = 99

highest_score = score1 if score1 > score2 else score2
```

Good:

```python
score1 = 90
score2 = 99

highest_score = max(score1, score2)
```

## FURB137: `simplify-comprehension`

Categories: `builtin` `iterable` `readability`

Often times generator expressions and list/set/dict comprehensions can be
written more succinctly. For example, passing a list comprehension to a
function when a generator expression would suffice, or using the shorthand
notation in the case of `list` and `set`. For example:

Bad:

```python
nums = [1, 1, 2, 3]

nums_times_10 = list(num * 10 for num in nums)
unique_squares = set(num ** 2 for num in nums)
number_tuple = tuple([num ** 2 for num in nums])
```

Good:

```python
nums = [1, 1, 2, 3]

nums_times_10 = [num * 10 for num in nums]
unique_squares = {num ** 2 for num in nums}
number_tuple = tuple(num ** 2 for num in nums)
```

## FURB138: `use-list-comprehension`

Categories: `performance` `readability`

When constructing a new list it is usually more performant to use a list
comprehension, and in some cases, it can be more readable.

Bad:

```python
nums = [1, 2, 3, 4]
odds = []

for num in nums:
    if num % 2:
        odds.append(num)
```

Good:

```python
nums = [1, 2, 3, 4]
odds = [num for num in nums if num % 2]
```

## FURB139: `no-multiline-strip`

Categories: `readability`

If you want to define a multi-line string but don't want a leading/trailing
newline, use a continuation character ('\') instead of calling `lstrip()`,
`rstrip()`, or `strip()`.

Bad:

```python
"""
This is some docstring
""".lstrip()

"""
This is another docstring
""".strip()
```

Good:

```python
"""\
This is some docstring
"""

"""\
This is another docstring\
"""
```

## FURB140: `use-starmap`

Categories: `itertools` `performance`

If you only want to iterate and unpack values so that you can pass them
to a function (in the same order and with no modifications), you should
use the more performant `starmap` function:

Bad:

```python
scores = [85, 100, 60]
passing_scores = [60, 80, 70]

def passed_test(score: int, passing_score: int) -> bool:
    return score >= passing_score

passed_all_tests = all(
    passed_test(score, passing_score)
    for score, passing_score
    in zip(scores, passing_scores)
)
```

Good:

```python
from itertools import starmap

scores = [85, 100, 60]
passing_scores = [60, 80, 70]

def passed_test(score: int, passing_score: int) -> bool:
    return score >= passing_score

passed_all_tests = all(starmap(passed_test, zip(scores, passing_scores)))
```

## FURB141: `use-pathlib-exists`

Categories: `pathlib`

When checking whether a file exists or not, try and use the more modern
`pathlib` module instead of `os.path`.

Bad:

```python
import os

if os.path.exists("filename"):
    pass
```

Good:

```python
from pathlib import Path

if Path("filename").exists():
    pass
```

## FURB142: `no-set-for-loop`

Categories: `builtin`

When you want to add/remove a bunch of items to/from a set, don't use a for
loop, call the appropriate method on the set itself.

Bad:

```python
sentence = "hello world"
vowels = "aeiou"
letters = set(sentence)

for vowel in vowels:
    letters.discard(vowel)
```

Good:

```python
sentence = "hello world"
vowels = "aeiou"
letters = set(sentence)

letters.difference_update(vowels)
```

## FURB143: `no-default-or`

Categories: `logical` `readability`

Don't check an expression to see if it is falsey then assign the same
falsey value to it. For example, if an expression used to be of type
`int | None`, checking if the expression is falsey would make sense,
since it could be `None` or `0`. But, if the expression is changed to
be of type `int`, the falsey value is just `0`, so setting it to `0`
if it is falsey (`0`) is redundant.

Bad:

```python
def is_markdown_header(line: str) -> bool:
    return (line or "").startswith("#")
```

Good:

```python
def is_markdown_header(line: str) -> bool:
    return line.startswith("#")
```

## FURB144: `use-pathlib-unlink`

Categories: `pathlib`

When removing a file, use the more modern `Path.unlink()` method instead of
`os.remove()` or `os.unlink()`: The `pathlib` module allows for more
flexibility when it comes to traversing folders, building file paths, and
accessing/modifying files.

Bad:

```python
import os

os.remove("filename")
```

Good:

```python
from pathlib import Path

Path("filename").unlink()
```

## FURB145: `no-slice-copy`

Categories: `readability`

Don't use a slice expression (with no bounds) to make a copy of something,
use the more readable `.copy()` method instead:

Bad:

```python
nums = [3.1415, 1234]
copy = nums[:]
```

Good:

```python
nums = [3.1415, 1234]
copy = nums.copy()
```

## FURB146: `use-pathlib-is-funcs`

Categories: `pathlib`

Don't use the `os.path.isfile` (or similar) functions, use the more modern
`pathlib` module instead:

Bad:

```python
if os.path.isfile("file.txt"):
    pass
```

Good:

```python
if Path("file.txt").is_file():
    pass
```

## FURB147: `no-path-join`

Categories: `pathlib`

When joining strings to make a filepath, use the more modern and flexible
`Path()` object instead of `os.path.join`:

Bad:

```python
with open(os.path.join("folder", "file"), "w") as f:
    f.write("hello world!")
```

Good:

```python
from pathlib import Path

with open(Path("folder", "file"), "w") as f:
    f.write("hello world!")

# even better ...

with Path("folder", "file").open("w") as f:
    f.write("hello world!")

# even better ...

Path("folder", "file").write_text("hello world!")
```

Note that this check is disabled by default because `Path()` returns a Path
object, not a string, meaning that the Path object will propogate throught
your code. This might be what you want, and might encourage you to use the
pathlib module in more places, but since it is not a drop-in replacement it
is disabled by default.

## FURB148: `no-ignored-enumerate-items`

Categories: `builtin`

Don't use `enumerate` if you are disregarding either the index or the
value:

Bad:

```python
books = ["Ender's Game", "The Black Swan"]

for index, _ in enumerate(books):
    print(index)

for _, book in enumerate(books):
    print(book)
```

Good:

```python
books = ["Ender's Game", "The Black Swan"]

for index in range(len(books)):
    print(index)

for book in books:
    print(book)
```

## FURB149: `no-bool-literal-compare`

Categories: `logical` `readability` `truthy`

Don't use `is` or `==` to check if a boolean is True or False, simply
use the name itself:

Bad:

```python
failed = True

if failed is True:
    print("You failed")
```

Good:

```python
failed = True

if failed:
    print("You failed")
```

## FURB150: `use-pathlib-mkdir`

Categories: `pathlib`

Use the `mkdir` method from the pathlib library instead of using the
`mkdir` and `makedirs` functions from the `os` library: the pathlib library
is more modern and provides better flexibility over the construction and
manipulation of file paths.

Bad:

```python
import os

os.mkdir("new_folder")
```

Good:

```python
from pathlib import Path

Path("new_folder").mkdir()
```

## FURB151: `use-pathlib-touch`

Categories: `pathlib`

Don't use `open(x, "w").close()` if you just want to create an empty file,
use the less confusing `Path.touch()` method instead.

Bad:

```python
open("file.txt", "w").close()
```

Good:

```python
from pathlib import Path

Path("file.txt").touch()
```

This check is disabled by default because `touch()` will throw a
`FileExistsError` if the file already exists, and (at least on Linux) it
sets different file permissions, meaning it is not a drop-in replacement.
If you don't care about the file permissions or know that the file doesn't
exist beforehand this check may be for you.

## FURB152: `use-math-constant`

Categories: `math` `readability`

Don't hardcode math constants like pi, tau, or e, use the `math.pi`,
`math.tau`, or `math.e` constants respectively.

Bad:

```python
def area(r: float) -> float:
    return 3.1415 * r * r
```

Good:

```python
import math

def area(r: float) -> float:
    return math.pi * r * r
```

## FURB153: `simplify-path-constructor`

Categories: `pathlib` `readability`

The Path() constructor defaults to the current directory, so don't pass the
current directory (".") explicitly.

Bad:

```python
file = Path(".")
```

Good:

```python
file = Path()
```

## FURB154: `simplify-global-and-nonlocal`

Categories: `builtin` `readability`

The `global` and `nonlocal` keywords can take multiple comma-separated
names, removing the need for multiple lines.

Bad:

```python
def some_func():
    global x
    global y

    print(x, y)
```

Good:

```python
def some_func():
    global x, y

    print(x, y)
```

## FURB155: `use-pathlib-stat`

Categories: `pathlib`

Don't use the `os.path.getsize` (or similar) functions, use the more modern
`pathlib` module instead:

Bad:

```python
if os.path.getsize("file.txt"):
    pass
```

Good:

```python
if Path("file.txt").stat().st_size:
    pass
```

## FURB156: `use-string-charsets`

Categories: `readability` `string`

Python includes some pre-defined charsets such as digits (0-9), upper and
lower case alpha characters, and so on. You don't have to define them
yourself, and they are usually more readable.

Bad:

```python
digits = "0123456789"

if c in digits:
    pass

if c in "0123456789abcdefABCDEF":
    pass
```

Good:

```python
if c in string.digits:
    pass

if c in string.hexdigits:
    pass
```

Note that when using a literal string, the corresponding `string.xyz` value
must be exact, but when used in an `in` comparison, the characters can be
out of order since `in` will compare every character in the string.

## FURB157: `simplify-decimal-ctor`

Categories: `decimal`

Under certain circumstances the `Decimal()` constructor can be made more
succinct.

Bad:

```python
if x == Decimal("0"):
    pass

if y == Decimal(float("Infinity")):
    pass
```

Good:

```python
if x == Decimal(0):
    pass

if y == Decimal("Infinity"):
    pass
```

## FURB158: `simplify-as-pattern-with-builtin`

Categories: `pattern-matching` `readability`

When pattern matching builtin classes such as `int()` and `str()`, don't
use an `as` pattern to bind to the value, since the most common builtin
classes can use positional patterns instead.

Bad:

```python
match x:
    case str() as name:
        print(f"Hello {name}")
```

Good:

```python
match x:
    case str(name):
        print(f"Hello {name}")
```

## FURB159: `simplify-strip`

Categories: `readability` `string`

In some situations the `.lstrip()`, `.rstrip()` and `.strip()` string
methods can be written more succinctly: `strip()` is the same thing as
calling both `lstrip()` and `rstrip()` together, and all the strip
functions take an iterable argument of the characters to strip, meaning
you don't need to call strip methods multiple times with different
arguments, you can just concatenate them and call it once.

Bad:

```python
name = input().lstrip().rstrip()

num = "  -123".lstrip(" ").lstrip("-")
```

Good:

```python
name = input().strip()

num = "  -123".lstrip(" -")
```

## FURB160: `no-redundant-assignment`

Categories: `readability`

Sometimes when you are debugging (or copy-pasting code) you will end up
with a variable that is assigning itself to itself. These lines can be
removed.

Bad:

```python
name = input("What is your name? ")
name = name
```

Good:

```python
name = input("What is your name? ")
```

## FURB161: `use-bit-count`

Categories: `builtin` `performance` `python310` `readability`

Python 3.10 adds a very helpful `bit_count()` function for integers which
counts the number of set bits. This new function is more descriptive and
faster compared to converting/counting characters in a string.

Bad:

```python
x = bin(0b1010).count("1")

assert x == 2
```

Good:

```python
x = 0b1010.bit_count()

assert x == 2
```

## FURB162: `simplify-fromisoformat`

Categories: `datetime` `python311` `readability`

Python 3.11 adds support for parsing UTC timestamps that end with `Z`, thus
removing the need to strip and append the `+00:00` timezone.

Bad:

```python
date = "2023-02-21T02:23:15Z"

start_date = datetime.fromisoformat(date.replace("Z", "+00:00"))
```

Good:

```python
date = "2023-02-21T02:23:15Z"

start_date = datetime.fromisoformat(date)
```

## FURB163: `simplify-math-log`

Categories: `math` `readability`

Use the shorthand `log2` and `log10` functions instead of passing 2 or 10
as the second argument to the `log` function. If `math.e` is used as the
second argument, just use `math.log(x)` instead, since `e` is the default.

Bad:

```python
power = math.log(x, 10)
```

Good:

```python
power = math.log10(x)
```

## FURB164: `no-from-float`

Categories: `decimal` `fractions` `readability`

When constructing a Fraction or Decimal using a float, don't use the
`from_float()` or `from_decimal()` class methods: Just use the more consice
`Fraction()` and `Decimal()` class constructors instead.

Bad:

```python
ratio = Fraction.from_float(1.2)
score = Decimal.from_float(98.0)
```

Good:

```python
ratio = Fraction(1.2)
score = Decimal(98.0)
```

## FURB165: `no-temp-class-object`

Categories: `readability`

You don't need to construct a class object to call a static method or a
class method, just invoke the method on the class directly:

Bad:

```python
cwd = Path().cwd()
```

Good:

```python
cwd = Path.cwd()
```

## FURB166: `use-int-base-zero`

Categories: `builtin` `readability`

When converting a string starting with `0b`, `0o`, or `0x` to an int, you
don't need to slice the string and set the base yourself: just call `int()`
with a base of zero. Doing this will autodeduce the correct base to use
based on the string prefix.

Bad:

```python
num = "0xABC"

if num.startswith("0b"):
    i = int(num[2:], 2)
elif num.startswith("0o"):
    i = int(num[2:], 8)
elif num.startswith("0x"):
    i = int(num[2:], 16)

print(i)
```

Good:

```python
num = "0xABC"

i = int(num, 0)

print(i)
```

This check is disabled by default because there is no way for Refurb to
detect whether the prefixes that are being stripped are valid Python int
prefixes (like `0x`) or some other prefix which would fail if parsed using
this method.

## FURB167: `use-long-regex-flag`

Categories: `readability` `regex`

Regex operations can be changed using flags such as `re.I`, which will make
the regex case-insensitive. These single-character flag names can be harder
to read/remember, and should be replaced with the longer aliases so that
they are more descriptive.

Bad:

```python
if re.match("^hello", "hello world", re.I):
    pass
```

Good:

```python
if re.match("^hello", "hello world", re.IGNORECASE):
    pass
```

## FURB168: `no-isinstance-type-none`

Categories: `pythonic` `readability`

Checking if an object is `None` using `isinstance()` is un-pythonic: use an
`is` comparison instead.

Bad:

```python
x = 123

if isinstance(x, type(None)):
    pass
```

Good:

```python
x = 123

if x is None:
    pass
```

## FURB169: `no-is-type-none`

Categories: `pythonic` `readability`

Don't use `type(None)` to check if the type of an object is `None`, use an
`is` comparison instead.

Bad:

```python
x = 123

if type(x) is type(None):
    pass
```

Good:

```python
x = 123

if x is None:
    pass
```

## FURB170: `use-regex-pattern-methods`

Categories: `readability` `regex`

If you are passing a compiled regular expression to a regex function,
consider calling the regex method on the pattern itself: It is faster, and
can improve readability.

Bad:

```python
import re

COMMENT = re.compile(".*(#.*)")

found_comment = re.match(COMMENT, "this is a # comment")
```

Good:

```python
import re

COMMENT = re.compile(".*(#.*)")

found_comment = COMMENT.match("this is a # comment")
```

## FURB171: `no-single-item-in`

Categories: `iterable` `readability`

Don't use `in` to check against a single value, use `==` instead:

Bad:

```python
if name in ("bob",):
    pass
```

Good:

```python
if name == "bob":
    pass
```

## FURB172: `use-suffix`

Categories: `pathlib`

When checking the file extension for a Path object don't call
`endswith()` on the `name` field, directly check against `suffix` instead.

Bad:

```python
from pathlib import Path

def is_markdown_file(file: Path) -> bool:
    return file.name.endswith(".md")
```

Good:

```python
from pathlib import Path

def is_markdown_file(file: Path) -> bool:
    return file.suffix == ".md"
```

Note: The `suffix` field will only contain the last file extension, so
don't use `suffix` if you are checking for an extension like `.tar.gz`.
Refurb won't warn in those cases, but it is good to remember in case you
plan to use this in other places.

## FURB173: `use-dict-union`

Categories: `dict` `readability`

Dicts can be created/combined in many ways, one of which is the `**`
operator (inside the dict), and another is the `|` operator (used outside
the dict). While they both have valid uses, the `|` operator allows for
more flexibility, including using `|=` to update an existing dict.

See PEP 584 for more info.

Bad:

```python
def add_defaults(settings: dict[str, str]) -> dict[str, str]:
    return {"color": "1", **settings}
```

Good:

```python
def add_defaults(settings: dict[str, str]) -> dict[str, str]:
    return {"color": "1"} | settings
```

## FURB174: `simplify-token-function`

Categories: `readability` `secrets`

Depending on how you are using the `secrets` module, there might be more
expressive ways of writing what it is you're trying to write.

Bad:

```python
random_hex = token_bytes().hex()
random_url = token_urlsafe()[:16]
```

Good:

```python
random_hex = token_hex()
random_url = token_urlsafe(16)
```

## FURB175: `simplify-fastapi-query`

Categories: `fastapi` `readability`

FastAPI will automatically pass along query parameters to your function, so
you only need to use `Query()` when you use params other than `default`.

Bad:

```python
@app.get("/")
def index(name: str = Query()) -> str:
    return f"Your name is {name}"
```

Good:

```python
@app.get("/")
def index(name: str) -> str:
    return f"Your name is {name}"
```

## FURB176: `unreliable-utc-usage`

Categories: `datetime`

Because naive `datetime` objects are treated by many `datetime` methods
as local times, it is preferred to use aware datetimes to represent times
in UTC.

This check affects `datetime.utcnow` and `datetime.utcfromtimestamp`.

Bad:

```python
from datetime import datetime

now = datetime.utcnow()
past_date = datetime.utcfromtimestamp(some_timestamp)
```

Good:

```python
from datetime import datetime, timezone

datetime.now(timezone.utc)
datetime.fromtimestamp(some_timestamp, tz=timezone.utc)
```

## FURB177: `no-implicit-cwd`

Categories: `pathlib`

If you want to get the current working directory don't call `resolve()` on
an empty `Path()` object, use `Path.cwd()` instead.

Bad:

```python
cwd = Path().resolve()
```

Good:

```python
cwd = Path.cwd()
```