# Programming Lesson 2

## Initial words

Firstly I want to point out a couple of things in the hello world program we wrote before.

* `print` is a Python command (actually a function call but you will learn more about that below).
* `'hello world'` is a 'string' which is usually used to represent Text values. We will come across other kinds of values in this lesson and over the next few lessons.

## Consecutive steps

Normally a Computer will do the steps in the order that you present.

For example, type in following and run it in Thonny.

```
print(1)
print(2)
print(3)
print(4)
print(5)
```

What do you get when you run this?

```
1
2
3
4
5
```

So the Computer printed out 5 numbers because that is what we asked it to do.

Notice that we can think of this problem in a different way.

1. First we need to do something 5 times
2. Secondly each time the number has to go up by 1
3. Thirdly we need to print the number every time.

This way of thinking is what you need to become a programmer. You take a problem and find ways to express it that is in an 'algorithmic' way, like creating a formula. The advantage of the formula is that it can work not just with this number problem but any problem of the same type.

For example, above we printed 1 to 5. But we could also print 5 to 15, or 10 to 150. We don't want to have to write multiple lines of `print` every time we want to print some numbers. Instead we want to say somthing like:

```
please_print(1,5)
```

Where we are saying please print 1 to 5.
Or we could say:

```
please_print(10,150)
```

So how do we do this?

