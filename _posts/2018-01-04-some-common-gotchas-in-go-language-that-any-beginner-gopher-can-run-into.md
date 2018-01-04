---
layout: post
title: 'Some Common GOTCHAS in GO LANGUAGE that any beginner Gopher can run into'
date: '2018-01-04 12:50:36'
desc: 'Some Common GOTCHAS in GO LANGUAGE that any beginner Gopher can run into'
keywords: 'Nethserver,Linux,Webserver,Server'
categories:
  - Golang
tags:
  - GO
  - GO Language
  - GOLANG
  - Programming
  - Learning Go Language
icon: icon-go
---
First thing is first. Happy New Years 🎉🎉

Now that’s out of the way, let’s talk about Go. I recently finished learning my basic perliminary real Go program. The process was quite fun and I learned a lot about Go in the process. So, to wrap up my first official foray into Rob Pike’s mystical land of gophers, I decided to write down some of the common “Gotchas!” that any beginning Gopher - like me - can run into.

<p align="center"><img src="{{ site.img_path }}/golang/golang-1.jpg" alt="Gophers can be quite aggressive sometimes" height="auto" width="40%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>

> **Gophers** can be quite aggressive sometimes

&nbsp;
# **⚠️ #1. Range**
----------
The range function is one of the most commonly used functions in Go. Here’s a sample use case of the range function. Note that for some demented reason, we decided to make all the animals in the zoo have 999 legs.

```golang
    type Animal struct {
    	name string
    	legs int
    }
    
    func main() {
      zoo := []Animal{ Animal{ "Dog", 4 },
                       Animal{ "Chicken", 2 },
                       Animal{ "Snail", 0 },
                     }
    
      fmt.Printf("-> Before update %v\n", zoo)
    
      for _, animal := range zoo {
        // 🚨 Oppps! `animal` is a copy of an element 😧
        animal.legs = 999
      }
    
      fmt.Printf("\n-> After update %v\n", zoo)
    }
```

The above code looks innocent enough. However, you may be surprised to find that the two fmt.Printf() statements yield the same results.

    -> Before update [{Dog 4} {Chicken 2} {Snail 0}]
    -> After update 🚨🚨🚨 [{Dog 4} {Chicken 2} {Snail 0}]

## **Lesson**

> Value property of range (stored here as animal) is a **copy of the value
> from** zoo, **not a pointer to the value in** zoo.

## **🛠️ The Fix**

In order to modify an element within the array, we must change the element via its **pointer.**

```golang
    for idx, _ := range zoo {
      zoo[idx].legs = 999
    }
```

This may look quite trivial but you may be surprised to find this as a one of the most common source of bugs; at least for me!

[» Go playground #1 for you to play around in](https://play.golang.org/p/jhL_MNbXnPC)

&nbsp;
# **⚠️ #2. The … Thingy**
----------
You may have used the … keyword in the C programming language to create a variadic function; [variadic function](https://www.gnu.org/software/libc/manual/html_node/Variadic-Functions.html) is a function that takes a variable number or type of arguments.

In C, you have to successively call the va_arg macro in order to access the optional arguments. And if you use the variadic argument in any other way, the compiler will throw an error.

```golang
    int add_em_up (int count,...) {
      ...
      va_start (ap, count);         /* Initialize the argument list */
      for (i = 0; i < count; i++)
          sum += va_arg(ap, int);   /* Get the next argument value */
      va_end (ap);                  /* Clean up */
      return sum
    }
```

In Go however, things are similar but quite different at the same time. Here is a variadic function myFprint in Go. Notice how the variadic argument a is being used.


```golang
    func myFprint(format string, a ...interface{}) {
      if len(a) == 0 {
        fmt.Printf(format)
      } else {
        // ⚠️ `a` should be `a...`
        fmt.Printf(format, a)
        // ✅
        fmt.Printf(format, a...)
      }
    }
    
    func main() {
        myFprint("%s : line %d\n", "file.txt", 49)
    }
```

Output:

    [file.txt %!s(int=49)] : line %!d(MISSING)
    file.txt : line 49

You’d think that the compiler would throw an error here for using the variadic parameter a in a wrong way. But notice how fmt.Sprintf just used the first argument in a without throwing a fit.

## **Lesson**

> In Go, **variadic parameters are converted to slices by the compiler**

[» Go playground #2 for you to play around in](https://play.golang.org/p/303g8_1IVFD)

&nbsp;
# **⚠️ #3. Slicing**
----------

If you have done your fair share of slicing in Python, you may remember that slicing in Python gives you a new list with just the references to the elements copied over. This property allows for code like this in Python.

```golang
    a = [1, 2, 3]
    b = a[:2]			# 👀 a completely new list!
    b[0] = 999
    >>> a
    [1, 2, 3]
    >>> b
    [999, 2]
```

However if you try the same thing in Go, you get something else.

```golang
    func main() {
      data := []int{1,2,3}
      slice := data[:2]
      slice[0] = 999
    
      fmt.Println(data)
      fmt.Println(slice)
    }
```

Output:

    [999 2 3]
    [999 2]

## **Lesson**

> In Go, **a slice shares the same backing array and capacity as the
> original.** So if you change an element in the slice, the original
> contents are modified as well.

## **🛠️ The Fix**

```golang
    If you want to get an independent slice, you have two options.
    // Option #1
    // appending elements to a nil slice
    // `...` changes slice to arguments for the variadic function `append`
    a := append([]int{}, data[:2]...)
    
    // Option #1
    // Create slice with length of 2
    // copy(dest, src)
    a := make([]int, 2)
    copy(a, data[:2])
```

And according to StackOverflow, the append option is slightly faster than the `make. + copy` option!

[» Go playground #3 for you to play around in](https://play.golang.org/p/HvVFmQZTcjp)
