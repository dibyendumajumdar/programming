# Values and Memory

The Computer has in-built temporary memory called RAM which is used heavily by all computer programs. Programs store values temporarily
in memory while doing work. 

The memory is basically a large area that is made up of 'bytes'. Think of the 'byte' as a unit of computer memory. Typically each 'byte' 
contains 8 'bits', where a bit is something that is either '1' or '0'. 

We can also combine bytes to make larger items such as:

* 'word' - typically made of 4 bytes
* 'quad word' - typically made of 8 bytes
* 'double' - represents decimal number, usually takes 8 bytes.
And so on.

But while the computer deals with 'bits', 'bytes', etc. programers prefer to not worry about this. We like to work with values that are
meaningful to us such as:

* Integer value - a number without decimal point
* Boolean value - a value that has two possible outcomes; True or False
* String values - such as 'Hello world' - this usually represents Text
* Decimal values - numbers that have a decimal point.

We also like to deal with composite values that are made up of other values. Examples are:

* List or Array of values - such as `[1, 2, 3, 4]`
* Dictionary or Map of values

When we write programs in Python, we use values such as above, and Pythn translates these to what the Computer understands.

For example:

```
fullname = 'Dibyendu Majumdar'
age = 10
```

In the above example, we stored two values in memory.
We gave names to the memory where we stored the values, i.e. `age` and `fullname`. 
We don't care where these are stored in memory, all we care is that we can access the value in memory when we need to.
Python will ensure that the values get stored somehere appropriate in memory.

To access the value in memory we simply refer to the name.

Example:

```
print(age)
print(fullname)
```
