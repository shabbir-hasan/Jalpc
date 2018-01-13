---
layout: post
title: 'Common GOTCHAS in Python Strings'
date: '2018-01-13 16:00:21'
desc: 'Common GOTCHAS in Python Strings'
keywords: 'Python,Strings'
categories:
  - Python
tags:
  - Python
  - Strings
  - Python-Strings
  - Programming
  - Learning Python
icon: icon-python
---

<p align="center"><img src="{{ site.img_path }}/python/python-strings/screenshot.png" alt="Common GOTCHAS in Python Strings" height="auto" width="50%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>

Recently out of curiosity, I was set to explore how strings are implemented in CPython. The results were overwhelming. I realized that how little I knew about different string concepts and optimizations in Python.

What's more fascinating is that a lot of programmers are unaware of these concepts. With these insights, I have an opportunity to baffle a few Python programmers out there.

Okay, I'll begin with a simple snippet that I ran in my IPython interpreter,

```python
    	>>> a = "wtf"
	>>> b = "wtf"
	>>> a is b
	True

	>>> a = "wtf!"
	>>> b = "wtf!"
	>>> a is b
	False

	>>> a, b = "wtf!", "wtf!"
	>>> a is b
	True
```

Makes sense, right?

Well if it doesn't, hold on. Everything will feel obvious after a while. But before we do so, let me throw my next snippet at you.

```python
	>>> a = "some_string"
	>>> id(a)
	140420665652016
	>>> id("some" + "_" + "string") # Notice that both the ids are same.
	140420665652016
```

Alright, one final attack before we take a break.

```python
	>>> 'a' * 20 is 'aaaaaaaaaaaaaaaaaaaa'
	True
	>>> 'a' * 21 is 'aaaaaaaaaaaaaaaaaaaaa'
	False
```

None of these outcomes would have made sense to me a couple of weeks ago, and seeing them, I'd have faced an existential crisis as a regular Python programmer.

<p align="center"><img src="{{ site.img_path }}/python/python-strings/screenshots.png" alt="I Am OUT!" height="auto" width="40%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>

Anyway, if you have no idea what's going on here, every outcome is just a consequence of a concept called "String interning" here.

> **Wait, what is this "string interning"?**

As strings are immutable objects in Python. It's possible for multiple variables to reference the same string object to save memory rather than creating a new object every time.

Python sometimes implicitly interns strings. The decision of when to implicitly intern a string is implementation dependent. However, some facts can make it easy for us to guess if a string will be interned or not:

* All length 0 and length 1 strings are interned.

* Strings are interned at compile time (```'wtf'``` will be interned but ```''.join(['w', 't', 'f']``` will not be interned).

* Strings that are not composed of ASCII letters, digits or underscores, are not interned. This explains why ```'wtf!'``` was not interned due to !.

**Oh I see, but why was ```"wtf!``` interned in ```a, b = "wtf!", "wtf!"```**

Well, when `a` and `b` are set to `"wtf!"` in the same line, the Python interpreter creates a new object, then references the second variable at the same time. If you do it on separate lines, it doesn't "know" that there's already `wtf!` as an object (because `"wtf!"` is not implicitly interned as per the facts mentioned above).

It's a compiler optimization and specifically applies to the interactive environment. When you enter two lines in a live interpreter, they're compiled separately, and therefore optimized independently. If you were to try this example in a .py file, you would not see the same behavior, because the file is compiled all at once.

Now just go through those snippets again. It all seems obvious now, doesn't it? That's the beauty of Python! ❤️

&nbsp;
# Wait, there's more!!!
----------

Yes, peace won't come easy. Here's another snippet:

```python
	>>> print("\\ some string \\")
	>>> print(r"\ some string")
	>>> print(r"\ some string \")

		File "<stdin>", line 1
		print(r"\ some string \")
								^
	SyntaxError: EOL while scanning string literal
```

What happened there?

Actually, in a raw string literal, as indicated by the prefix r, the backslash doesn't have the special meaning. What the interpreter actually does, though, is simply change the behavior of backslashes, so they pass themselves and the following character through. That's why backslashes don't work at the end of a raw string.

**Here's one more..**

```python
	>>> print('wtfpython''')
	wtfpython
	>>> print("wtfpython""")
	wtfpython
	>>> # The following statements raise `SyntaxError`
	>>> # print('''wtfpython')
	>>> # print("""wtfpython")
```

Can you guess why `print('''wtfpython')` or `print("""wtfpython")` would raise a "SyntaxError" here?

If your answer is no, It's time to introduce a new concept called "implicit string concatenation." Python supports implicit string literal concatenation.

For example:

```python
	>>> print("wtf" "python")
	wtfpython
	>>> print("wtf" "") # or "wtf"""
	wtf
```

`'''` and `"""` are also string delimiters in Python, which causes a `SyntaxError` because the Python interpreter was expecting a terminating triple quote as delimiter while scanning the currently encountered triple quoted string literal.

**Moar..** (it's the last, pinky promise)

```python
	# using "+", three strings:
	>>> timeit.timeit("s1 = s1 + s2 + s3", setup="s1 = ' ' * 100000; s2 = ' ' * 100000; s3 = ' ' * 100000", number=100)
	0.25748300552368164
	# using "+=", three strings:
	>>> timeit.timeit("s1 += s2 + s3", setup="s1 = ' ' * 100000; s2 = ' ' * 100000; s3 = ' ' * 100000", number=100)
	0.012188911437988281
```

Notice the stark difference in execution times of `s1 += s2 + s3` and `s1 = s1 + s2 + s3`? `+=` is actually faster than `+` for concatenating more than two strings because the first string (example, `s1` for `s1 += s2 + s3`) is not destroyed while calculating the complete string.



Before I conclude, here's a quick fact, `'a'[0][0][0][0][0]` is a semantically valid statement in Python. Why? Because strings are immutable [sequences](https://docs.python.org/3/glossary.html#term-sequence) (iterables supporting element access using integer indices) in Python. The implementation of the above concepts and optimizations in Python is possible due to this very fact.

Alright, here we end. I hope you find this post interesting and informative. If you would like to learn about more such hidden Python gems, I'd recommend you check out [What the f**ck Python?](https://github.com/satwikkansal/wtfpython) which is a curated collection of such subtle and tricky snippets.

*[Originially Published at codementor.io](https://www.codementor.io/satwikkansal/do-you-really-think-you-know-strings-in-python-fnxh8mtha)*