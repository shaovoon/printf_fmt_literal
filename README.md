## TIP: printf() format string should be a constant string literal

`printf()` format string argument should be a constant string literal, not dynamic string from user input. Because it poises a security problem if the format string can be supplied/modified by a malicious actor. Consider this example.

```Cpp
char name[] = "bar";

printf("Name : %s\n", name);
```

The output is as follows

```
Name : bar
```

Suppose the hacker can modify the format string's `%s` specifier to `%p` to display the memory address of `name` which is on the stack. Using the `name`'s stack address, he can cause a buffer overrun to modify the function's return address to an arbitrary address of his choice.

```Cpp
char name[] = "bar";

printf("Name : %p\n", name);
```

The output is as follows. Most likely than not, your memory address and mine are different.

```
Name : 0000002E380FFBF4
```

You can choose to use one of the several solutions below.

* Always use string literal for `printf`'s format string
* Use [C++20's std::format](https://en.cppreference.com/w/cpp/utility/format)
* Use [C++23's std::print](https://en.cppreference.com/w/cpp/io/print)
* Use [fmt](https://github.com/fmtlib/fmt) which is the reference implementation for [C++20's std::format](https://en.cppreference.com/w/cpp/utility/format) and [C++23's std::print](https://en.cppreference.com/w/cpp/io/print)
* Last but not least, if you are on C++11, use [ValuesWriter](https://github.com/shaovoon/values_writer)