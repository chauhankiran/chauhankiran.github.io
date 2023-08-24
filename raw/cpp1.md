# C++ Saga Pt. 1

The journey of the programming language starts with _hello, world_ program. I don't want to make any exception here. So, here it is - the _hello, world_ program in C++. Create a file with name `hello.cpp` and write the following code.

```cpp
#include<iostream>

int main() {
	std::cout << "hello, world" << std::endl;

	return 0;
}
```

Let's first run this program and then I'll explain you the code by write this _hello, world_ program again from the scratch.

To run this program, we need C++ compiler. If you're using GNU/Linux, lucky you that you have `g++` or GNU C++ compiler. If you're using Debian (or Ubuntu or Mint), you can run the following command to install the `g++` compiler.

```bash
$ sudo apt install g++
```

Once the installation is done, run the following command to confirm the installation.

```bash
$ g++ --version
g++ (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0
Copyright (C) 2021 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

If you see the output like this, congratulation, g++ is installed on your computer successfully!

To run the `hello.cpp` program, following command is used.

```bash
$ g++ hello.cpp -o hello
```

We're invoking `g++` compiler on `hello.cpp` file and telling compiler to generate and executable(`-o`) with name `hello`.

g++ will execute the code written in `hello.cpp` and _silently_ create an executable `hello`. You can run this executable by prefixing `./` as follows.

```bash
$ ./hello
hello, world
```

---

Now that the code is working, it is the time that I should explain the code.

Go ahead and delete the code written in `hello.cpp` file and also delete the `hello` executable.

What we're interested to do? Print the _hello, world_ text in the terminal. So, let's write the _hello, world_ text in the file `hello.cpp`.

```cpp
hello, world
```

This _hello, world_ is the text. Text or _string_ in C++ should be within the double quotes. Single quotes are reserved for character. So, let's write the _hello, world_ text with double quotes.

```cpp
"hello, world"
```

This string is not going to be printed in the terminal by itself. We need to instruct. We can instruct using `cout` and special symbol `<<`.

```cpp
cout << "hello, world"
```

This means, _hello, world_ is going to streaming out the the text in console. But, this `cout` is not directly available for us to use in the C++ program.  First we need to include `iostream` header at the top.

```cpp
#include<iostream>

cout << "hello, world"
```

By convention, when you include `iostream`, everything within `iostream` is available under the name (or namespace) `std`. So, in order to use the `cout` from `std`, we need to write it as `std::cout`. 

```cpp
#include<iostream>

std::cout << "hello, world"
```

C++ is open of those language that requires the semicolon to terminate the statement.

```cpp
#include<iostream>

std::cout << "hello, world";
```

The execution of the C++ program starts from `main()` function. If we want to print _hello, world_ text, we must need to invoke or write within `main()` function otherwise it wouldn't run.

```cpp
#include<iostream>

main() {
	std::cout << "hello, world";
}
```

This `main()` function should return an integer. For now, let's explicitly return integer `0` at the end of the function. This is for compiler to not complain about the return value from `main()` function.

```cpp
#include<iostream>

int main() {
	std::cout << "hello, world";

	return 0;
}
```

And this is our hello, world program. You know how to run this program, right?

```bash
$ g++ hello.cpp -o hello
$ ./hello
hello, world
```

Ah! We need to add a new line at the end to pretty print the _hello, world_ text in the terminal. Let's use `std::endl` or end line after the _hello, world_ text.

```cpp
#include<iostream>

int main() {
	std::cout << "hello, world" << std::endl;

	return 0;
}
```

Let's run the program and confirm the output.

```bash
$ g++ hello.cpp -o hello
$ ./hello
hello, world
```

And we have the working hello, world program again in C++.

Don't worry if some of the explanation my confuse you. After couple of articles and few weeks of practices all started to make sense. Just don't procrastinate!